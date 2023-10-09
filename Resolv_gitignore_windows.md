# Reoslution problème gitignore non reconnu par git sous windows

Il se peut qu'en créant le fichier .gitignore via la commande `echo "" > .gitignore` sous windows, le fichier sois mal encodé et non reconnu par git (git status renvoie pleins de fichiers qui doivent etre commit)

Pour résoudre:

Dans powershell écrire:
```ps
function Get-FileEncoding {
    param ( [string] $FilePath )

    [byte[]] $byte = get-content -Encoding byte -ReadCount 4 -TotalCount 4 -Path $FilePath

    if ( $byte[0] -eq 0xef -and $byte[1] -eq 0xbb -and $byte[2] -eq 0xbf )
        { $encoding = 'UTF8' }  
    elseif ($byte[0] -eq 0xfe -and $byte[1] -eq 0xff)
        { $encoding = 'BigEndianUnicode' }
    elseif ($byte[0] -eq 0xff -and $byte[1] -eq 0xfe)
         { $encoding = 'Unicode' }
    elseif ($byte[0] -eq 0 -and $byte[1] -eq 0 -and $byte[2] -eq 0xfe -and $byte[3] -eq 0xff)
        { $encoding = 'UTF32' }
    elseif ($byte[0] -eq 0x2b -and $byte[1] -eq 0x2f -and $byte[2] -eq 0x76)
        { $encoding = 'UTF7'}
    else
        { $encoding = 'ASCII' }
    return $encoding
}

```

* et ```Get-FileEncoding .gitignore```

* Si la fonction renvoie autre chose que ASCII ou UTF-8:
* ```Set-Content .gitignore -Encoding Ascii -Value (Get-Content .gitignore)```
* ```git status``` 