---
layout: default
---

# HƯỚNG DẪN SỬ DỤNG GCLOUD SHELL

## TẢI GCLOUD SHELL TRÊN WINDOWS

Mở cmd hoặc powershell lên để tiến hành tải thư viện SDK của GCloud

```powershell
(New-Object Net.WebClient).DownloadFile("https://dl.google.com/dl/cloudsdk/channels/rapid/GoogleCloudSDKInstaller.exe", "$env:Temp\GoogleCloudSDKInstaller.exe")

& $env:Temp\GoogleCloudSDKInstaller.exe
```

## CÀI ĐẶT

Mở cmd hoặc powershell lên để tiến hành cài đặt **Alpha**

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

## XEM THÊM (NGUỒN CHÍNH)

- [Cloud Shell Online](https://ssh.cloud.google.com/cloudshell/editor)
- [Installation GCloud SDK](https://cloud.google.com/sdk/install?hl=it)