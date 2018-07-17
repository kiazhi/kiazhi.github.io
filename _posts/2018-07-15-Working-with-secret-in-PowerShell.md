---
layout: single
title: "Working with secret in PowerShell"
excerpt: "A guide on how to convert plain text String to SecureString and
reverse the encrypted SecureString back to the original plain text String in
PowerShell."
author: Ryen Tang
header:
  image: ./assets/images/banners/PowerShell-1280x200.png
date: 2018-07-15 00:00:00 +1200
toc: true
categories: 
  - Blog
  - PowerShell
tags:
  - Blog
  - PowerShell
  - Security
  - Cryptography
  - ".Net Framework"
  - "Ryen Tang"
---

Have you been given a task to automate some jobs that require some kind of
credential authentication? I certainly have a lot of those in the past till
today where the script requires a username and password value to proceed with
the job. And it gets complicated when it requires a script calling another
script that may not be developed by yourself.

## Storing sensitive information as variable 

How do you store sensitive information as variable in PowerShell? This is a
very common task in scripting where you may requires to store that password
value as a variable for repurposing it. But storing password value as a
variable means that the variable value is in clear text which is not ideal.

That is why there is a `ConvertTo-SecureString` cmdlet available that assist
you in securing plain text `String` to become a `SecureString` type prior to
storing it as a variable in memory.

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### Converting plain text String to SecureString type

Let us try to use `ConvertTo-SecureString` cmdlet to convert a plain text
`String` value of "MyTopSecretPassword" as a `SecureString` and store it as a
variable in a script scope. We will also attempt to get the stored variable to
output on to the console. 

```powershell
$SCRIPT:INSECUREPASSWORD = 'MyTopSecretPassword'

Write-Information `
    -MessageData "What is my insecure Password? $SCRIPT:INSECUREPASSWORD" `
    -InformationAction 'Continue'

$SCRIPT:SECUREPASSWORD = ConvertTo-SecureString `
    -String 'MyTopSecretPassword' `
    -AsPlainText `
    -Force

Write-Information `
    -MessageData "What is my secure Password? $SCRIPT:SECUREPASSWORD" `
    -InformationAction 'Continue'
```

If you have copied the code above and run them on your PowerShell console, you
will find that `$SCRIPT:INSECUREPASSWORD` variable will return plain text value
of your password and `$SCRIPT:SECUREPASSWORD` variable will only returns the
type name of the variable on the console. Looks pretty secure to me.

**Output:**
```text
What is my insecure Password? MyTopSecretPassword
What is my secure Password? System.Security.SecureString
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### Converting SecureString to String type

Now that we know `$SCRIPT:SECUREPASSWORD` is a `SecureString` type, can I
convert it back to a `String` type? Yes, you can convert a `SecureString` type
using `ConvertFrom-SecureString` cmdlet as below:

```powershell
$SCRIPT:SECUREPASSWORD | ConvertFrom-SecureString
```

Unfortunately, the output is basically not your plain text password `String`,
instead it returns an encrypted `String` and therefore is basically not usable
in some cases where you may want the `SecureString` to returns back the
original plain text password `String` value for configuring a non-Microsoft
products.

> Note: The output below will not be identical with your output due to
encryption.

**Output:**
```text
01000000d08c9ddf0115d1118c7a00c04fc297eb01000000594d2a0d29aca7458ab4d8da81a486b
e0000000002000000000003660000c0000000100000006e036855074752e50aedd3fd08707aa200
00000004800000a000000010000000753569fec88f896d534f933af15f4659280000006211ae529
05857e099cccb8f81f50bf7159e1db255e6926ed481ce850273ae84526fc20002f534d914000000
45209f670a1af769dd5e23b50be6450d6ea93252
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

### Converting encrypted String to plain text String

Since an encrypted `String` is not something that we can use, we will need to
use the encrypted `String` value that we got and unencrypt them back to plain
text `String` value.

If you are using PowerShell that still utilises old .Net Framework
libraries, you will have to use `Marshal` class from
`System.Runtime.InteropServices` namespace with `PtrToStringAuto` and
`SecureStringToBSTR` methods to convert the encrypted `String` as below.

```powershell
# .NET FRAMEWORK 2.0
[System.Runtime.InteropServices.Marshal]::PtrToStringAuto( `
    [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR( `
    (ConvertTo-SecureString `
                -String $($SCRIPT:SECUREPASSWORD | ConvertFrom-SecureString) `
                -Force)))
```

If you are using PowerShell that utilises newer .Net Framework
libraries, you will have to use `Marshal` class from
`System.Runtime.InteropServices` namespace with `PtrToStringAnsi` method, and
`SecureStringMarshal` class from `System.Security` namespace with
`SecureStringToCoTaskMemAnsi` methods to convert the encrypted `String` as
below.

```powershell
# .NET FRAMEWORK 4.7.1
[System.Runtime.InteropServices.Marshal]::PtrToStringAnsi( `
    [System.Security.SecureStringMarshal]::SecureStringToCoTaskMemAnsi( `
        (ConvertTo-SecureString `
            -String $($SCRIPT:SECUREPASSWORD | ConvertFrom-SecureString) `
            -Force)))
```

> Note: The above methods do not work on PowerShell Core 6.0 of higher for
non-Windows platforms such as Linux and macOS due to issue
[#1654](https://github.com/PowerShell/PowerShell/issues/1654) with
`ConvertFrom-SecureString` cmdlet on those platforms since 5 Aug 2016. As such,
we may have to wait until that issue
[#1654](https://github.com/PowerShell/PowerShell/issues/1654) is resolved
or a workaround is available for those platforms. In short, we are still brain
storming about it.

And you will obtain your original plain text password value outputs on the
console as below.

**Output:**

```text
MyTopSecretPassword
```

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## Conclusion

To summarize this, it is possible to secure a plain text `String` into a
`SecureString` that is encrypted and it is also possible to reverse the
encrypted `SecureString` back to the original plain text `String` value in
PowerShell. Does that mean it is insecure to store any sensitive values as
`SecureString`? No, it is still very secure to some extent because someone
will need to be able to hack into the machine in advance to run a
malicious process with access to raw memory to reverse the encrypted
`SecureString`. Secondly, it is impossible to copy the `SecureString` (long)
encrypted `String` value and reverse it from another machine due to a mismatch
cryptography cipher key. For more information, please kindly refer to [How secure is SecureString?](https://msdn.microsoft.com/en-us/library/system.security.securestring(v=vs.110).aspx#HowSecure)

Why is there two different ways of converting encrypted `String` back to plain
text `String`? That is because PowerShell on older operating system is
using legacy .Net Framework. Since then Microsoft has been improving their
.Net Framework and there are a few namespaces and classes rearrangement.
Therefore, if you are not aware of how to do this with newer .Net Framework
previously, I hope you know how to do it now and enjoyed reading this. If you
reckon this is good information, help share this with others and keep them
informed.

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>

## References

- [System.Runtime.InteropServices.Marshal.PtrToStringAuto()](https://msdn.microsoft.com/en-us/library/ewyktcaa%28v=vs.110%29.aspx)
- [System.Runtime.InteropServices.Marshal.SecureStringToBSTR()](https://msdn.microsoft.com/en-us/library/system.runtime.interopservices.marshal.securestringtobstr(v=vs.110).aspx)
- [System.Runtime.InteropServices.Marshal.PtrToStringAnsi()](https://msdn.microsoft.com/en-us/library/7b620dhe(v=vs.110).aspx)
- [System.Security.SecureStringMarshal.SecureStringToCoTaskMemAnsi()](https://msdn.microsoft.com/en-us/library/system.security.securestringmarshal.securestringtocotaskmemansi(v=vs.110).aspx)

<hr style='margin-top: 0.5em; margin-bottom: 0em; border-top: 1px solid #eaeaea'>
<p style='font-size: 16px; vertical-align: top; text-align: right;'>↑<a href='#top'>Top</a></p>
