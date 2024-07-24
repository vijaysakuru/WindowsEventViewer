# pip install pywinrm

# winrm set winrm/config/service/auth '@{Basic="true"}'

# winrm set winrm/config/service '@{AllowUnencrypted="true"}'

## Env SetUP
    python3 -m venv venv
    source venv/bin/activate  # On macOS or Linux


## To run the app in normal Flask buit-in Server
    python run.py
    http://127.0.0.1:5000/

## To run the app in normal Flask and WSGI Server
    gunicorn "run:app"
    http://127.0.0.1:5000/

## WinRM Setup
    winrm quickconfig

### Configure WinRM to Use HTTPS
    $cert = New-SelfSignedCertificate -DnsName "your-server-name" -CertStoreLocation Cert:\LocalMachine\My

    New-Item -Path WSMan:\LocalHost\Listener -Transport HTTPS -Address * -CertificateThumbPrint $cert.Thumbprint -Force

### Open Firewall Ports
    New-NetFirewallRule -Name "WinRM HTTP" -DisplayName "WinRM HTTP" -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 5985

    New-NetFirewallRule -Name "WinRM HTTPS" -DisplayName "WinRM HTTPS" -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 5986

## Configure WinRM Service
    Set-Service -Name WinRM -StartupType Automatic
    Start-Service -Name WinRM


### Adjust Authentication Methods (Optional)
    winrm set winrm/config/service/auth '@{Basic="true"}'

    winrm set winrm/config/service/auth '@{Negotiate="false"}'

### Kerberos Authentication
    winrm set winrm/config/service/auth '@{Kerberos="true"}'

    winrm set winrm/config/service '@{AllowEncryption="true"}'

https://www.hurryupandwait.io/blog/understanding-and-troubleshooting-winrm-connection-and-authentication-a-thrill-seekers-guide-to-adventure

https://pitstop.manageengine.com/portal/en/kb/articles/troubleshooting-winrm-errors#ErrorA_specified_logon_session_does_not_exist_It_may_already_have_been_terminated

https://www.hurryupandwait.io/blog/fixing-winrm-firewall-exception-rule-not-working-when-internet-connection-type-is-set-to-public



