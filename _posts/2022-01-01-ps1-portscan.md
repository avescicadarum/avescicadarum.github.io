---
layout: post
title:  "powershell - portscan"
date:   2022-01-01
categories: windows
---

## Powershell - Port Scan


```powershell

$ips = ("10.10.10.1","10.10.10.2");
$ports = (21, 22, 53, 80, 88, 445, 1433, 3389, 5985, 8080);

foreach ($ip in $ips) {

    if (Test-Connection -BufferSize 32 -Count 1 -ComputerName $ip -Quiet) {
        Write-Host "[+] The "$ip" is Online"
        Write-Host "[!] Port Scan starting ..."
        foreach ($port in $ports) {
            try {

                $socket = New-Object System.Net.Sockets.TcpClient($ip, $port);

            }
            catch {

            };

            if ($socket -eq $null) {
                Write-Host $ip":"$port" - Closed";

            }
            else {

               Write-Host $ip":"$port" - Open";
               $socket = $null;
            }
        }
    }
    else {
        Write-Host "[-] The "$ip" is Down"
    }
}
```