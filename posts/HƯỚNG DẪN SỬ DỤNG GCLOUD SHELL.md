---
layout: default
---

# CÁC ĐƯỜNG DẪN

- [Cloud Shell Online](https://ssh.cloud.google.com/cloudshell/editor)
- [Installation GCloud SDK](https://cloud.google.com/sdk/install?hl=it)

# TẢI GCLOUD SHELL TRÊN WINDOWS

```powershell
(New-Object Net.WebClient).DownloadFile("https://dl.google.com/dl/cloudsdk/channels/rapid/GoogleCloudSDKInstaller.exe", "$env:Temp\GoogleCloudSDKInstaller.exe")

& $env:Temp\GoogleCloudSDKInstaller.exe
```

# LỆNH CƠ BẢN VÀO GClOUD Shell

## CÀI ĐẶT

```bash
gcloud alpha
```

## VÀO ALPHA SHELL

```bash
gcloud alpha cloud-shell ssh
```

Hoặc các bạn có thể xem thêm bằng lệnh "help":

```bash
gcloud alpha --help 
```