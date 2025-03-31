
**<h2>Wi-Fi WPA2 Key Extractor GUI</h2>**


A modern, animated Python GUI tool for capturing WPA2 handshakes and cracking Wi-Fi passwords using aircrack-ng utilities. Designed for ethical hacking, educational use, and cybersecurity research.
________________________________________
**🚀 Features**

●	Dark-themed PyQt5 interface

●	One-click Wi-Fi scan using nmcli

●	Automatic best-client deauthentication using aireplay-ng

●	Live handshake capture via airodump-ng

●	Key cracking with aircrack-ng

●	Real-time progress bars and status indicators

●	Built-in Wi-Fi restoration feature

________________________________________
**📁 Project Structure**


project_root/

├── wpa2_gui.py                                                       # Main GUI script

├── README.md                                                         # Project readme

├── wpa2_extractor_sdd.md                                             # Software design document

├── scmp.md                                                           # Configuration management plan

├── performance_risk_analysis.md                                      # Performance testing & risk assessment

├── wordlists/

│   └── rockyou.txt.gz                                                # Example password wordlist

└── .git/                                                             # Version control directory

________________________________________
**🛠️ Requirements**

●	Python 3.6+

●	Kali Linux or compatible Debian-based distro

●	External USB Wi-Fi adapter (monitor mode capable)

●	Tools:

    ○	aircrack-ng

    ○	nmcli

    ○	sudo

Python Dependencies:

sudo apt install python3-pyqt5 aircrack-ng network-manager

________________________________________
**🧪 How to Run**

sudo python3 wpa2_gui.py

🔒 Must be run with root privileges for packet injection and monitor mode.
________________________________________
**📊 Performance & Testing**

●	Scans networks in < 10s

●	GUI stays responsive using background threads

●	Tracks CPU/RAM usage

●	Documented test cases in performance_risk_analysis.md

________________________________________
**⚠️ Legal Disclaimer**

This tool is intended for educational and authorized security testing only. Unauthorized use on networks you don’t own or have permission to test is illegal.
Use responsibly. By using this tool, you agree to these terms.
________________________________________



