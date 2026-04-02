A working exploit for Chrome Beta 81.0.4044.69 on Windows x64 is included in the attachments.

Two possible patches are also included. One has InstalledAppProviderImpl inherit from WebContentsObserver to observe the render frame destruction and clear its reference. The other patch has InstalledAppProviderImpl store the (process_id, frame_id) pair in place of the raw pointer and perform dynamic lookup of the rfh.

VERSION
Chrome Version: 81.0.4044.0+
Operating System: Desktop

REPRODUCTION CASE

Attached as a zip because the MojoJS generated bindings are included. See poc.html for a minimal trigger and pwn.html for an exploit. The exploit appends --no-sandbox to the Browser process's command line object, causing all new subprocesses to run unsandboxed.

Instructions:
Unzip gIRA.zip and serve from an HTTP server, e.g. on localhost port 8080.

PoC:
Run chrome with --enable-blink-features=MojoJS,MojoJSTest and visit http://localhost:8080/trigger.html

Exploit:
Run chrome beta (81.0.4044.69) with --enable-blink-features=MojoJS,MojoJSTest. Find base address of chrome.dll (e.g. using windbg) and replace the value of kChromeDllBase in pwn.html. (Note that this base address could be acquired from a compromised renderer). Then visit http://localhost:8080/pwn.html. After the exploit completes, open a new tab and check the process listing to confirm the subprocess is running with --no-sandbox.

CREDIT INFORMATION
Reporter credit: Tim Becker of Theori
