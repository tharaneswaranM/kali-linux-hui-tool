# kali-linux-hui-tool
🛠️ Kali Linux GUI Toolkit – By Tharaneswaran
A modern Windows 11–style GUI hacking tool built using CustomTkinter for ethical hackers, red teamers, and cybersecurity enthusiasts.
It integrates all major Kali Linux tools under one roof with a smooth GUI experience.
🚀 Features

    ✅ Windows 11–style blurred matte design with a green loading splash screen

    ✅ Dynamic tool execution with target input prompts

    ✅ Categories:

        🔍 Scanning & Enumeration

        🔑 Password Attacks

        🛡️ Exploitation

        🧠 Information Gathering

        🕸️ Web App Testing

        📡 Network Attacks

        🧪 Malware Analysis

        📁 Forensics

        🖼️ GUI Preview

    .
├── theam.py               # Main launcher script
├── win11_blur.png         # Background image (Optional, used in UI)
└── README.md              # This documentation





import customtkinter as ctk
import subprocess
import threading
import time
import os
from PIL import Image

ctk.set_appearance_mode("dark")
ctk.set_default_color_theme("green")

class Tool:
    def __init__(self, name, command_template, emoji, description):
        self.name = f"{emoji} {name}"
        self.command_template = command_template
        self.description = description

categories = {
    "🔍 Scanning & Enumeration": [
        Tool("Nmap", "nmap {target}", "🧭", "Network scanner to discover hosts and services."),
        Tool("FFUF", "ffuf -u http://{target}/FUZZ -w /usr/share/wordlists/dirb/common.txt", "🌀", "Fast web fuzzer for directories and vhosts."),
        Tool("Gobuster", "gobuster dir -u http://{target} -w /usr/share/wordlists/dirb/common.txt", "🚪", "Directory and DNS brute-forcer."),
        Tool("WPScan", "wpscan --url http://{target}", "📝", "WordPress vulnerability scanner.")
    ],
    "🔑 Password Attacks": [
        Tool("John the Ripper", "john {target}", "🔨", "Password cracker using wordlists or hashes."),
        Tool("Hydra", "hydra -L users.txt -P passwords.txt {target} http-get", "🐍", "Brute force network logins."),
        Tool("Medusa", "medusa -h {target} -U users.txt -P passwords.txt -M ssh", "👹", "Parallel, modular login brute-forcer."),
        Tool("Hashcat", "hashcat -a 0 -m 0 {target} /usr/share/wordlists/rockyou.txt", "🐱", "High-performance password cracker.")
    ],
    "🛡️ Exploitation & Post-Exploitation": [
        Tool("Metasploit", "msfconsole -q -x 'use exploit/multi/handler'", "💣", "Exploit development and post-exploitation platform."),
        Tool("Searchsploit", "searchsploit {target}", "🔍", "Search Exploit-DB for vulnerabilities.")
    ],
    "🧠 Information Gathering": [
        Tool("WhatWeb", "whatweb {target}", "🌐", "Identify websites' technologies."),
        Tool("theHarvester", "theHarvester -d {target} -l 100 -b google", "🍴", "Gather emails, subdomains, and hosts."),
        Tool("Recon-ng", "recon-ng", "🧠", "Web reconnaissance framework.")
    ],
    "🕸️ Web Application Tools": [
        Tool("Nikto", "nikto -h {target}", "🕷️", "Web server scanner for vulnerabilities."),
        Tool("XSStrike", "xsstrike -u http://{target}", "⚔️", "Advanced XSS detection suite."),
        Tool("SQLmap", "sqlmap -u http://{target} --batch", "🧪", "Automated SQL injection tool."),
        Tool("Sublist3r", "sublist3r -d {target}", "📜", "Subdomain enumeration tool."),
        Tool("Dirb", "dirb http://{target}", "🗂️", "Web content scanner.")
    ],
    "📡 Network Attacks": [
        Tool("Ettercap", "ettercap -T -i eth0 -M arp:remote /{target}// /gateway//", "🎭", "MITM attack tool."),
        Tool("Bettercap", "bettercap -eval 'net.probe on'", "⚡", "Powerful modular network attack framework."),
        Tool("Aircrack-ng", "aircrack-ng {target}", "📶", "Wi-Fi network cracker.")
    ],
    "🧪 Malware Analysis": [
        Tool("Radare2", "r2 {target}", "🔬", "Open-source reverse engineering framework."),
        Tool("Cutter", "cutter", "✂️", "GUI for radare2 reverse engineering."),
        Tool("Ghidra", "ghidraRun", "🐉", "NSA's software reverse engineering tool.")
    ],
    "📁 Forensics Tools": [
        Tool("Autopsy", "autopsy", "🧩", "Digital forensics platform for hard drives and smartphones."),
        Tool("Binwalk", "binwalk {target}", "🪜", "Firmware analysis tool for extracting embedded files."),
        Tool("Foremost", "foremost -i {target} -o output", "🗃️", "File carving tool to recover deleted files."),
        Tool("Volatility", "volatility -f {target} --profile=Win7SP1x64 pslist", "🧠", "Memory forensics framework.")
    ]
}

splash = ctk.CTk()
splash.overrideredirect(True)
splash.geometry("600x300+400+200")
splash.configure(bg="#111111")

splash_label = ctk.CTkLabel(splash, text="Single Tool for All – BY THARANESWARAN", font=("Segoe UI", 22, "bold"), text_color="#00FF99")
splash_label.pack(pady=50)

progress = ctk.CTkProgressBar(splash, width=400)
progress.pack(pady=10)
progress.set(0)

progress_label = ctk.CTkLabel(splash, text="0%", font=("Segoe UI", 14))
progress_label.pack()

for i in range(101):
    progress.set(i/100)
    progress_label.configure(text=f"{i}%")
    splash.update()
    time.sleep(0.02)

splash.destroy()

def run_tool(command):
    def thread_func():
        subprocess.run(command, shell=True)
    threading.Thread(target=thread_func).start()

def show_tool_info(tool):
    def submit():
        ip = entry.get()
        if ip:
            run_tool(tool.command_template.format(target=ip))
        info_window.destroy()

    info_window = ctk.CTkToplevel()
    info_window.title(f"{tool.name} Info")
    info_window.geometry("500x300")

    ctk.CTkLabel(info_window, text=f"{tool.description}", wraplength=450).pack(pady=10)
    ctk.CTkLabel(info_window, text="Enter Target/IP:").pack(pady=(20, 5))
    entry = ctk.CTkEntry(info_window, width=400)
    entry.pack(pady=5)
    ctk.CTkButton(info_window, text="Run", command=submit).pack(pady=10)

app = ctk.CTk()
app.title("🔧 Kali Linux GUI Toolkit - By Tharaneswaran")
app.geometry("950x720")

if os.path.exists("win11_blur.png"):
    bg_img = Image.open("win11_blur.png")
    bg = ctk.CTkImage(light_image=bg_img, size=(950, 720))
    bg_label = ctk.CTkLabel(app, image=bg, text="")
    bg_label.place(relx=0.5, rely=0.5, anchor="center")
else:
    print("⚠️ Background image 'win11_blur.png' not found. Skipping background.")

scroll_frame = ctk.CTkScrollableFrame(app, width=900, height=700, corner_radius=25, fg_color="#111111")
scroll_frame.place(relx=0.5, rely=0.5, anchor="center")

for category, tools in categories.items():
    ctk.CTkLabel(scroll_frame, text=category, font=("Segoe UI", 20, "bold"), text_color="cyan").pack(anchor="w", pady=(10, 0))
    for tool in tools:
        ctk.CTkButton(scroll_frame, text=tool.name, command=lambda t=tool: show_tool_info(t),
                      corner_radius=15, fg_color="#1f6aa5", hover_color="#2f81c7", text_color="white", font=("Segoe UI", 14, "bold")).pack(anchor="w", padx=20, pady=5)

app.mainloop()
