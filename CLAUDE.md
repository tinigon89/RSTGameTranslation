# CLAUDE.md

## Quy tắc branch (bắt buộc)

- **Chỉ làm việc trên branch `jk-dev`.** Mọi thay đổi code, commit đều nằm trên `jk-dev`.
- **`main` chỉ dùng để lấy code mới nhất từ source gốc**, không commit trực tiếp lên `main`.
- Remote:
  - `origin` → `tinigon89/RSTGameTranslation` (fork, dùng gh account `tinigon89`)
  - `upstream` → `thanhkeke97/RSTGameTranslation` (source gốc)

### Cập nhật code mới từ source gốc

```bash
git checkout main
git pull --ff-only upstream main
git push origin main
git checkout jk-dev
git merge main
```

## Dự án

RSTGameTranslation: app WPF dịch màn hình game theo thời gian thực (OCR + dịch AI + overlay, speech-to-text, TTS).

- .NET 9, `net9.0-windows10.0.19041.0`, WPF + WinForms, `win-x64`, `Nullable` bật
- Solution: `RST.sln`, project: `RST.csproj` (assembly `rst`, root namespace `WPFScreenCapture`)
- Toàn bộ source nằm trong `src/`; output build ra `app/`

## Build

```bash
dotnet build RST.sln
dotnet publish ./RST.csproj -c Release -r win-x64 --self-contained true
```

Đóng gói release: `MakeReleaseZip.bat`.

## Lưu ý

- `RST.csproj` tắt default items, chỉ build `src\**\*.cs` và `src\**\*.xaml`: **file code mới phải đặt trong `src/`**, nếu không sẽ không được build.
- Không bật `PublishSingleFile`: làm hỏng việc load ONNX Runtime (crash khi dùng Supertonic TTS).
