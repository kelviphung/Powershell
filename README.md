***
# The script create by Kelvin Phung from AAC to work . When copying or using for personal purposes, please cite the author's copyright.
# Active account administrator and set password  
    $pwd = Read-Host " Input your password in here"  -AsSecureString
    Get-LocalUser | Where-Object {$_.Name -eq "Administrator"} | ForEach-Object {
    if ($_.Enabled -eq $true) {
    Write-Output " Local Administrator account already exists."
    } else {
    $_ | Enable-LocalUser
    $_ | Set-LocalUser -Password $pwd
    }
    }
# Identity Computer as such format AACVP 
## Check type asset 
``Function check_type
{
    $type = $false
    if(Get-WmiObject -Class win32_systemenclosure | 
        Where-Object { $_.chassistypes -eq 8 -or $_.chassistypes -eq 9 -or $_.chassistypes -eq 10 -or $_.chassistypes -eq 14 -or $_.chassistypes -eq 30})
        {
        return "LW"
        }
        Else
        {
        return "DW"
        }}
$c_host=check_type``
# Check serial number if great than 6 charter get last 6 charter, if serialnumber is empty input into "" 
``$SerialNumber = (Get-WmiObject -class win32_bios).SerialNumber``
# Get os ver
``$os= [System.Environment]::OSVersion.Version.Major``
# Input location
``$Title = "Asset location"``
``$Message = "Select from the list below your asset location?"``

``$VP = New-Object System.Management.Automation.Host.ChoiceDescription "&Vĩnh Phúc"``
``$BN = New-Object System.Management.Automation.Host.ChoiceDescription "&Bắc Ninh"``

``$loc = [System.Management.Automation.Host.ChoiceDescription[]]($VP, $BN)``

``$result = $host.ui.PromptForChoice($Title, $Message, $loc, 1)``

``Switch($result)
{
   0 {  $l_host= "APAVP" }
   1 { $l_host= "APAVN" }
}
$l_host``
# Make new hostname
``$n_host= $c_host+$os+$l_host+$SerialNumber.Substring($SerialNumber.Length -6)``
$n_host=[string]$n_host
# Join Computer to Domain.
``$old_host=hostname``
``Add-Computer -DomainName aac.com -ComputerName $old_host -NewName $n_host -Restart``
# This is ps1 file [Auto SetHostName ](https://drive.google.com/file/d/1uc1vKgyTPRgZ60aQm5Z9gAy87h3rKPAi/view?usp=drive_link)
