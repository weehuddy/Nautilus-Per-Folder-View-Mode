# Nautilus – Per-Folder View Mode Patch

This project is a custom build of Nautilus (GNOME Files) with an additional feature:
👉 Remember and restore per-folder view modes (List / Grid)

✨ What it does

Keeps track of the last view mode you used for each folder.

When you return to a folder, Nautilus automatically restores your preferred view.

Works seamlessly with existing Nautilus functionality — no new UI, just smarter behavior.

🔧 Why?

Upstream Nautilus only lets you set a global default view mode.
If you prefer certain folders (e.g. ~/Pictures) in Grid view and others (e.g. ~/Documents) in List view, you need to toggle manually each time.

# Build from Source 
wget https://download.gnome.org/sources/nautilus/48/nautilus-48.3.tar.xz 

tar xf nautilus-48.3.tar.xz

cd nautilus-48.3

wget https://raw.githubusercontent.com/weehuddy/Nautilus-Per-Folder-View-Mode-Patch/refs/heads/gnome-48/nautilus-restore-folder-view.patch

patch -p1 < ./nautilus-restore-folder-view.patch

sudo cp ./data/org.gnome.nautilus.gschema.xml /usr/share/glib-2.0/schemas/

sudo glib-compile-schemas /usr/share/glib-2.0/schemas

mkdir build && cd build

## Build & run (without installing) ###
meson setup --prefix=/usr --buildtype=release ..
            
ninja

nautilus --quit

./src/nautilus

## Build & install to system ###
meson setup --prefix=/usr ..

sudo ninja install


# Build from Debain Source ###
sudo apt-get build-dep nautilus

apt-get source nautilus

wget https://raw.githubusercontent.com/weehuddy/Nautilus-Per-Folder-View-Mode/refs/heads/main/nautilus-restore-folder-view.patch

cd nautilus-48.3

patch -p1 < ../nautilus-restore-folder-view.patch

dpkg-source --commit

DEB_BUILD_OPTIONS=nocheck dpkg-buildpackage -us -uc

sudo dpkg -i ../nautilus-data_48.3-2_all.deb 

sudo dpkg -i ../nautilus_48.3-2_amd64.deb

