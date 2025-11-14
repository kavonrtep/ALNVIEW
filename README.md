
# ALNview: A Qt-based alignment viewer for FASTGA

## _Author:  Gene Myers_
## _First:   April 10, 2025_

In brief, ALNview allows you to view the alignments in a .1aln as a "dot" plot between the two source
genomes, where each alignnment is viewed as a line segment.  You can view several different .1aln files that
are between the same two genomes as a set of layers that can be turned on an off.  For each layers
the thickness and color of the lines is under your control.  There is also a special layer that show
a true k-mer dot plot where you can control the size of k and the color of the dots.  This special
layer only becomes visible when the field of view in both dimensions is less than 1Mbp.

You can zoom by selecting regions or pressing up/down buttons. You can also pick alignment segments
which displays the coordinates, length, and iid of the alignment and gives you the option of requesting
to see the actual alignment in a secondary window.  And more ...

ALNview is currently only available as a prebuilt, binary .dmg for Apple computers.  We also give
you all the source files so the ambitious (or desperate :-) ) user can build it for other operating
systems using Qt 6.9.0 or higher.  Indeed if you make a binary image for a Windows or Unix machine
I would be very happy if you made a pull request with the relevant file and I will place it here.

---

## Building on Linux (Ubuntu/Debian)

### Prerequisites

Install required packages:

```bash
# Update package manager
sudo apt update

# Install Qt 6 development libraries
sudo apt install qt6-base-dev qt6-tools-dev

# Install build tools
sudo apt install build-essential libgl1-mesa-dev

# Install zlib development library
sudo apt install zlib1g-dev
```

**Minimum versions:**
- Qt 6.9.0 or higher
- GCC 9+ or Clang 10+
- zlib 1.2.11+

### Build Steps

1. **Navigate to the project directory:**
   ```bash
   cd /path/to/ALNVIEW
   ```

2. **Generate build files using qmake:**
   ```bash
   qmake6 viewer.pro
   ```


3. **Compile the project:**
   ```bash
   make -j$(nproc)
   ```

   The `-j$(nproc)` flag uses all available CPU cores for faster compilation.

4. **Run ALNview:**
   ```bash
   ./ALNview
   ```

### Troubleshooting

**Error: "qmake: command not found"**
- Install Qt tools: `sudo apt install qt6-tools-dev`
- Or use full path: `/usr/lib/qt6/bin/qmake viewer.pro`

**Error: "fatal error: zlib.h: No such file"**
- Install zlib development headers: `sudo apt install zlib1g-dev`

**Error: "Qt6Core not found"**
- Reinstall Qt development packages: `sudo apt install qt6-base-dev`

**Build fails with compiler errors**
- Ensure you have a compatible C++ compiler: `gcc --version` (requires GCC 9+)
- If using Clang: `clang --version` (requires Clang 10+)
- Install if needed: `sudo apt install gcc g++ gdb`

**qmake complains about missing dependencies**
- Clear build cache and regenerate:
  ```bash
  rm -rf BUILD/
  make clean
  qmake viewer.pro
  make
  ```

### Optional: Create a Desktop Application

To make ALNview available in your applications menu:

1. **Copy the binary to a standard location:**
   ```bash
   sudo cp ./ALNview /usr/local/bin/
   sudo chmod +x /usr/local/bin/ALNview
   ```

2. **Create a .desktop file:**
   ```bash
   cat > ~/.local/share/applications/alnview.desktop << 'EOF'
   [Desktop Entry]
   Type=Application
   Name=ALNview
   Exec=ALNview
   Icon=utilities-terminal
   Categories=Science;Biology;
   Terminal=false
   EOF
   ```

3. **Make it executable:**
   ```bash
   chmod +x ~/.local/share/applications/alnview.desktop
   ```

Now ALNview will appear in your application launcher.

### Optional: Build with Debug Symbols

For debugging purposes, modify `viewer.pro` to include debug information:

```makefile
# Add to viewer.pro:
CONFIG += debug
QMAKE_CXXFLAGS += -g -O0
QMAKE_CFLAGS += -g -O0
```

Then rebuild:
```bash
qmake viewer.pro
make clean
make
```

Run with gdb:
```bash
gdb ./ALNview
```
