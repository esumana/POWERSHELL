# POWERSHELL
move files across the network
Map a network drive locally. You must be authorized to browse the network and to access the machine. Credentials are essential.
List and filter items in a directory.
Drop pathname from the name of the file to be copied. 
Then copy and delete from source.
#Loop through the dir
Clear-Host  <br>
if (Get-PSDrive -Name P){<br>
  if ($?){<br>
      cd C:\Windows\System32\winevt\Logs<br>
      $hostname = $env:computername<br>
      $files = Get-ChildItem .\*Abc*<br>
      $a = 0<br>
      ForEach ($file in $files){<br>
          $outputFile = Split-Path $file -leaf #Get the file, not the path<br>
          echo $outputFile<br>
          Copy-Item -Path .\$outputFile -Destination \\fserver\log\$hostname-$outputFile<br>
          if ($?){<br>
              echo "YOU SUCCESSFULLY COPIED THE LAST FILE!"           <br>
              Remove-Item .\$outputFile   #Delete file if copy was successful<br>
          }<br>
      }<br>
    }<br>
<br>
  else {<br>
  $mycredentials = Get-Credential<br>
  New-PSDrive -Name "P" -PSProvider "FileSystem" -Root "\\fserver\log" -Credential $mycredentials<br>
  if ($?){<br>
        cd C:\Windows\System32\winevt\Logs<br>
        $hostname = $env:computername<br>
        $files = Get-ChildItem .\*Abc*<br>
        $a = 0<br>
        ForEach ($file in $files){<br>
          $outputFile = Split-Path $file -leaf #Get the file, not the path<br>
          echo $outputFile<br>
          #echo $hostname-$file+$a<br>
          #$a = $a + 1<br>
          Copy-Item -Path .\$outputFile -Destination \\fserver\log\$hostname-$outputFile <br>
          if ($?){<br>
              echo "YOU SUCCESSFULLY COPIED THE LAST FILE!"<br>
              #dir \\fserver\log            <br>
              Remove-Item .\$outputFile  #Delete file if copy was successful<br>
          }<br>
      }<br>
    }<br>
   }<br>
}<br>
else {<br>
    echo "Something went wrong!!!!!!!!!!!!!"<br>
}<br>
