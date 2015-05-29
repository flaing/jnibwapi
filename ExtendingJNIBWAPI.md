# Introduction #

This tutorial will outline step by step how to add a new function to the JNI interface, and recompile the client module in VS2008 Express. This example will create a simple print function to allow us access to BWAPI::Game->printf()

To get started, we'll need the following:
  * The latest version of JNIBWAPI (0.3 at the time of this writing)
  * A compatible version of BWAPI (3.7 is compatible with jnibwapi 0.3)

You can obtain JNIBWAPI from the downloads page, and BWAPI from [here](http://code.google.com/p/bwapi/downloads/list)

# Adding a new native method #

To begin with, we need to add the new method so that we can call it from our agent written in Java. Open JNIBWAPI.java in your favourite editor, and add the following line (I suggest in a new section near the declarations where you will put all of your new functions together).

`public native void printText(String text);`

Recompile your JNIBWAPI project.

# Generating the new C++ header from JNIBWAPI.java #

Included in the jnibwapi distributable or SVN is a simple batch file that will generate a C++ header containing the appropriate declarations. It works by reading from JNIBWAPI.java, so if you add a new native method to JNIBWAPI.java, and run the batch file, it will automatically generate the new C++ declarations for you.

This means that you **must not** edit the header file (jnibwapi\_JNIBWAPI.h) by hand - it is supposed to be machine generated from you Java class containing the interface.

To generate the new header file, simply run src/c/GenerateJNIHeader.bat

# Setting Up Visual Studio #
## Visual Studio 2010+ ##
Ensure the v90 Platform Toolset is installed on the system. The easiest way to do this is to install VS 2008 Express (and VS 2010 Express if you are using VS 2012).

Ensure the JAVA\_HOME environment variable exists and is set to a Java JDK folder (eg. C:\Program Files\Java\jdk1.7.0\_11)

Open src/c/client-bridge.vcxproj

## Visual Studio 2008 ##
**Note: This method is no longer recommended**

In order to recompile the client module (to add the new native method that we're going to use), it is suggested that you use VS2008 Express to compile the BWAPI project that we are about to assemble. At the time of this writing, it is the only IDE with official support from the BWAPI team.

To get started, we'll need to extract the contents of the BWAPI zip file (available on the BWAPI downloads page) to a folder. Remove the contents of BWAPI\_3.7/ExampleAIClient/Source/, and copy client-bridge.cpp and jnibwapi\_JNIBWAPI.h from jnibwapi/src/c/ to that folder. Rename client-bridge.cpp to ExampleAIClient.cpp.

Open ExampleProjects.sln in VS2008 Express. You'll see the various BWAPI example projects in the solution explorer. We are interested in ExampleAIClient.

First, we need to add the new header file that we generated to the project (Solution Explorer->ExampleAIClient->Header Files->Add->Existing Item->`ExampleAIClient/Source/jnibwapi_JNIBWAPI.h`).

Secondly, we're going to need to add the JNI header files from the JDK to our include path so that our project can include them.
(Solution Explorer->ExampleAIClient->Properties->C/C++->Additional Include Directories). You need to add your java/jdk1.7.0\_XX/include and java/jdk1.7.0\_XX/include/win32 folders to this list of directories.

Finally, we want to make sure that our ExampleAIClient project exports a dynamic library (.dll), not an executable (.exe). You can set this in (Solution Explorer->Properties->Configuration Properties->General->Configuration Type).

# Implementing the new function #

Open up jnibwapi\_JNIBWAPI.h and have a look at the interface declarations. We're interested in the one that we just added to the interface (printText). It should look like this:

```
/*
 * Class:     jnibwapi_JNIBWAPI.h
 * Method:    printText
 * Signature: (Ljava/lang/String;)V
 */
JNIEXPORT void JNICALL Java_jnibwapi_JNIBWAPI_printText
  (JNIEnv *, jobject, jstring);
```

We'll be implementing this function in client-bridge.cpp (or ExampleAIClient.cpp if you used the VS2008 method above).

```
JNIEXPORT void JNICALL Java_jnibwapi_JNIBWAPI_printText
  (JNIEnv * env, jobject jObj, jstring message)
{
	const char *messagechars = env->GetStringUTFChars(message, 0);
	Broodwar->printf(messagechars);
	env->ReleaseStringUTFChars(message, messagechars);
}
```

First, we need to convert the message string to an ASCII C-String, (java strings are unicode). That is why we make a call to the JNIEnv to convert it. Note that with integers (and I think floats and doubles too?) you will not need to make a call to the environment in order to convert them to their C++ equivalents - implicit conversion allows us to return a C++ int for a JNI function returning a jint, and use a jint in place of a C++ int in our C++ code, etc.

Secondly, we make the call to BWAPI that we wanted - printf - in order to display the string in Starcraft:Brood War. Finally, we call ReleaseStringUTFChars which just indicates to the JNI environment that the native method no longer needs the UTF-8 string returned by GetStringUTFChars, freeing up the memory allocated for string conversion.

Save, set the project output to "Release" and "Win32" (drop down box at the top of the Visual Studio window) and then build the project.

_If you followed the VS2008 method above_, copy the new client .dll from BWAPI/release/ to jnibwapi/release/ folder and rename it to client-bridge-x86.dll.

You should now be able to use the new native method in your JNIBWAPI project simply by making a call to that method eg: `bwapi.printText("Hello World!");`