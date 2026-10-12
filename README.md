# Introduction to Keylogging & Detection Guide

## 1. Introduction to Keylogging

**Simple:** Keylogging is secretly recording whatever a user types on a keyboard.

**Technical:** It is the process of intercepting `keyboard input events` at the OS level. The input from the hardware driver is captured before it reaches the target application using API hooking or kernel callbacks.

```mermaid
flowchart TD
    A[What is Keylogging?] --> B[Secretly Recording Keystrokes]
    B --> C[Why it Matters?]
    C --> C1[Passwords]
    C --> C2[Bank Details]
    C --> C3[Personal Chats]
    C1 --> D[Need for Awareness]
```

## 2. How Keylogging Works?

**Simple Flow:** User types -> OS receives it -> Someone in middle copies it -> Saves in memory -> Sends out.

**Technical Flow:** Physical Key Press -> Keyboard Driver generates Scancode -> Windows Input Stack -> Hook intercepts via SetWindowsHookEx -> Data stored in Buffer.

```mermaid
flowchart TD
    A[User Presses Key] --> B[Keyboard Driver<br/>Generates Scancode]
    B --> C[Operating System Input Stack]
    C --> D{Interception Point}
    D -->|User-Mode| E[API Hook<br/>SetWindowsHookEx / GetAsyncKeyState]
    D -->|Kernel-Mode| F[Kernel Callback]
    D -->|Browser Layer| G[JS Event Listener<br/>keydown event]
    E --> H[Temporary Buffer<br/>Memory Storage]
    F --> H
    G --> H
    H --> I[Obfuscation<br/>Base64 / XOR Encryption]
    I --> J[Exfiltration Attempt<br/>HTTPS / DNS]
```

## 3. Developing a Keylogger - Conceptual ArchitectureNote:

    Note: This section explains the conceptual modules for educational purpose, not the actual coding implementation.

A keylogger logically has 5 modules:

**1. Initialization Module:** How the program starts.

**2. Hooking Module:** How it listens to keyboard events. (Tech: WH_KEYBOARD_LL)

**3. Buffer Module:** How it stores data temporarily. (Tech: RAM / Temp File)

**4. Persistence Module:** How it stays active after restart. (Tech: Registry Run Key / Startup Folder) - Studied for detection.

**5. Exfiltration Module:** How data is sent out. (Tech: HTTP POST)

```mermaid
flowchart TD
    A[Developing a Keylogger<br/>Conceptual Architecture] --> B[1. Initialization<br/>Program Start]
    B --> C[2. Hook Installation<br/>Listen to Keystrokes]
    C --> D[3. Buffer Management<br/>Store in Memory]
    D --> E[4. Data Processing<br/>Filter Important Data]
    E --> F[5. Persistence Logic<br/>Lab Study Only]
    F --> G[6. Exfiltration Logic<br/>For Detection Study]
    G --> H[End - Analyzed in Lab]
```

## 4. How to Detect Keylogging?
# Level 1: Basic Detection
* Check Task Manager > Startup for unknown apps.
* Unusual keyboard lag or delay.
* Run full Antivirus / Windows Defender scan.

# Level 2: Technical Detection

```mermaid
flowchart TD
    A[Keystrokes in Buffer] --> B[Encrypted Data]
    B --> C[Tries to Send to Remote Server]
    C --> D{Detection Layer}
    D --> E[EDR Detection<br/>Detects SetWindowsHookEx<br/>API Call]
    D --> F[Network Detection<br/>Unusual Outbound Traffic<br/>to Unknown Domain]
    D --> G[Behavior Detection<br/>YARA Rule Match<br/>GetAsyncKeyState + WriteFile]
    E --> H[Blocked & Logged]
    F --> H
    G --> H
    H --> I[Alert Generated<br/>Quarantine & Investigation]
```
* **Process Level**: Use Autoruns (Sysinternals) to see hidden DLLs.
* **Network Level:** Use Wireshark or Firewall logs to see data going to unknown IP.
* **Rule Based:** YARA Rule flags file containing keylogging APIs + network APIs together.

## 5. How to Prevent?

1. Do not install cracked / unknown software.
2. Keep Windows Defender Real-time Protection ON.
3. Use 2FA / MFA - Even if password leaks, account stays safe.
4. Use On-Screen Keyboard for banking.
5. Keep Browser and OS updated.

## 6. Conclusion
Understanding keylogging is not to misuse it, but to understand how data theft happens and how we can defend against it. This guide is for awareness and defense.
