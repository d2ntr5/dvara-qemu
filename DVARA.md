# dvara-qemu

Dvara(맥에서 Windows 11 Arm 을 돌리는 개인용 앱)에 넣은 QEMU 의 **대응 소스**입니다. GPL-2.0 에 따라 공개합니다.
Corresponding source of the QEMU binaries shipped inside the Dvara app (GPL-2.0).

- 바탕: [utmapp/qemu](https://github.com/utmapp/qemu) `v10.0.12-utm` 그대로 (첫 커밋 「utmapp/qemu v10.0.12-utm (unmodified source)」)
- Dvara 수정 (두 번째 커밋):
  - Arm `virt` 의 `tpm-crb-device` 에 TPM PPI(물리적 존재 인터페이스) 추가 — 2026-03 Red Hat 이 `tpm-tis-device` 에 낸 패치 방식을 CRB 로 옮김. HVF 에서 RAM 매핑 대신 MMIO 영역으로. 윈도우 시스템 로그의 TPM 이벤트 15 가 사라지고 `Get-Tpm` 이 정상.
  - `ui/meson.build`: Metal SPICE 화면 코드는 OpenGL 이 켜졌을 때만 빌드
- 릴리즈 `qemu-10.0.12-dvara1`: 이 소스 묶음과, 앱에 같이 넣은 LGPL 라이브러리(glib · gettext/libintl · json-glib)의 같은 버전 원본 소스

## 빌드 (Apple Silicon 맥, Homebrew)

```
brew install glib pixman libslirp zstd spice-protocol ninja meson pkg-config
mkdir b && cd b
../configure --target-list=aarch64-softmmu --without-default-features --enable-hvf --enable-slirp --enable-vnc \
  --enable-pixman --enable-tools --enable-tpm --enable-zstd --enable-spice-protocol --disable-docs --disable-werror --enable-fdt=internal
make -j8 && make install
```

`qemu-system-aarch64` 은 `com.apple.security.hypervisor` 권한으로 서명해야 HVF 를 씁니다.
