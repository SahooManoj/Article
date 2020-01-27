# Introduction

This article will have a deep concept about the various ways of `encoding/encrypting` and storing password using PowerShell and how to use it in authentication. Basically to know how we can keep our credential secure. So let's start!

# Get-Credential

As we all know `Get-Credential` cmdlet is the most common way we use to pass credential in PowerShell script. This cmdlet is bydefault present in PowerShell and it is present inside the module `Microsoft.PowerShell.Security`.

Let see some examples of using `Get-Credential` and it's different role and functionality.

Example 1:

![Basic use of Get-Crendential](img/ex-1.1.png)

In the above screenshot I have run the `Get-Credential` cmdlet and as you can see it gives a pop up and asks for the `Username` and `Password` to be passed in the field. We will unable to see the password here as it is sensitive.

Once we pass the required credential and hit ok it will create a `PSCredential` object that will have 2 attribute as `Username` and `Password` which is shown in the below screenshot.

![](img/ex-1.2.png)

Here the password is encrypted and stored as `SecureString` which is a `.NET class`. This is done to keep the password confidential and it automatically get deleted from the memory when the session close.

we can store the credential in a variable to use it further in our script as shown below.

![](img/ex-1.3.png)

`$mycred` contains the credential and can be used for authentication. As we can see here the type of `$mycred` is `PSCredential` which proves that this is a `PSCredential` object.


As a developer, we may require to check the password complexity from our end. So in that case we need to convert the secure string to text readable format and then by using regex we can check the complexity. So let's take another example to see how we can decrypt the password from secure string to text.

Example 2:

```powershell
$mycred = Get-Credential
$password = $MyCred.password
$bstr = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($password) #converting securestring to byte string
$decryptPassword = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto($bstr) #converting byte string to string
[Runtime.InteropServices.Marshal]::ZeroFreeBSTR($BSTR) #recommended to free the byte string memory after conversion
```

Output:

![](img/ex-2.png)