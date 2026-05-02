# Tiny Pomodoro XFCE APT Repository

This repository hosts the public APT repository for [Tiny Pomodoro XFCE](https://github.com/guarinogio/tiny-pomodoro-xfce).

It contains the Debian package index, signed release metadata, public signing key, and `.deb` artifacts used by APT.

## Install

Add the signing key:

```bash
curl -fsSL https://guarinogio.github.io/tiny-pomodoro-xfce-apt/public.key \
  | sudo gpg --dearmor --yes -o /usr/share/keyrings/tiny-pomodoro-xfce.gpg
```

Add the APT source:

```bash
echo "deb [arch=all signed-by=/usr/share/keyrings/tiny-pomodoro-xfce.gpg] https://guarinogio.github.io/tiny-pomodoro-xfce-apt stable main" \
  | sudo tee /etc/apt/sources.list.d/tiny-pomodoro-xfce.list
```

Update package indexes:

```bash
sudo apt update
```

Install Tiny Pomodoro XFCE:

```bash
sudo apt install tiny-pomodoro-xfce
```

Run:

```bash
tiny-pomodoro
```

## Verify

Check the package source and candidate version:

```bash
apt policy tiny-pomodoro-xfce
```

Expected output should include this repository:

```text
https://guarinogio.github.io/tiny-pomodoro-xfce-apt stable/main all Packages
```

## Repository structure

```text
.
├── public.key
├── dists/
│   └── stable/
│       ├── InRelease
│       ├── Release
│       ├── Release.gpg
│       └── main/
│           └── binary-all/
│               ├── Packages
│               └── Packages.gz
└── pool/
    └── main/
        └── t/
            └── tiny-pomodoro-xfce/
                └── tiny-pomodoro-xfce_VERSION_all.deb
```

## Publishing

This repository is updated automatically from the main project repository:

https://github.com/guarinogio/tiny-pomodoro-xfce

When a version tag is pushed in the main project, GitHub Actions builds the Debian package, regenerates the APT metadata, signs the repository, and pushes the updated files here.

## Manual update

If needed, the repository can be regenerated manually:

```bash
mkdir -p dists/stable/main/binary-all
mkdir -p pool/main/t/tiny-pomodoro-xfce

cp ../tiny-pomodoro-xfce_VERSION_all.deb pool/main/t/tiny-pomodoro-xfce/

dpkg-scanpackages --arch all pool > dists/stable/main/binary-all/Packages
gzip -k -f dists/stable/main/binary-all/Packages

cat > dists/stable/Release <<'EOF'
Origin: Tiny Pomodoro XFCE
Label: Tiny Pomodoro XFCE
Suite: stable
Codename: stable
Architectures: all
Components: main
Description: APT repository for Tiny Pomodoro XFCE
EOF

apt-ftparchive release dists/stable >> dists/stable/Release

gpg --default-key KEY_ID --batch --yes --armor \
  --detach-sign \
  -o dists/stable/Release.gpg \
  dists/stable/Release

gpg --default-key KEY_ID --batch --yes --clearsign \
  -o dists/stable/InRelease \
  dists/stable/Release
```

## Notes

- Distribution: `stable`
- Component: `main`
- Architecture: `all`
- Package: `tiny-pomodoro-xfce`
- Repository URL: `https://guarinogio.github.io/tiny-pomodoro-xfce-apt`

## Project links

- Main project: https://github.com/guarinogio/tiny-pomodoro-xfce
- APT repository: https://github.com/guarinogio/tiny-pomodoro-xfce-apt

## License

The repository metadata is published for distribution of Tiny Pomodoro XFCE. The application itself is released under the MIT license.
