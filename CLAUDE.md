# Claude Instructions for TouchDesigner Project

## Pre-Response Requirements

**CRITICAL: Before answering ANY TouchDesigner-related question, you MUST:**

1. **Read this entire CLAUDE.md file** to understand the project context and requirements
2. **Reference the TouchDesigner documentation** for accurate parameter names and API usage:
   - Main UserGuide: https://derivative.ca/UserGuide
   - Python API Documentation: https://derivative.ca/UserGuide/Category%3APython
3. **Reference the AxiDraw documentation** when questions involve pen plotting:
   - AxiDraw Python API: https://axidraw.com/doc/py_api/#
   - Hardware: AxiDraw V3 with brushless servo configuration
4. **Consult the following project-specific files** for context about the HydraToTD system:
   - `Architechure.md` - System architecture and module specifications
   - `Hydra-TouchDesigner Implementation Blueprint.md` - Step-by-step build instructions and implementation details
   - `Hydra-TouchDesigner Technical Stack.md` - Technical specifications, dependencies, and performance requirements
   - `ProjectDescription.md` - Core features, workflow, and build priorities

## Documentation Reference Guidelines

### Always Verify Parameter Names
- **DO NOT guess** TouchDesigner parameter names or methods
- **ALWAYS check** the official documentation for exact parameter syntax
- **Common TouchDesigner objects** to reference:
  - `op()` - Operator references
  - `me` - Current operator context
  - `parent()` - Parent component access
  - `args[]` - Callback arguments
  - Parameter syntax: `op.par.parametername`

### Key TouchDesigner Python Classes to Reference:
- `OP` class - Base operator class
- `COMP` class - Component operators
- `TOP` class - Texture operators
- `CHOP` class - Channel operators
- `SOP` class - Surface operators
- `DAT` class - Data operators
- `MAT` class - Material operators

### Parameter Access Patterns:
```python
# Correct parameter access patterns to verify in docs:
op.par.parameter_name.val          # Get parameter value
op.par.parameter_name = value       # Set parameter value
op.par.parameter_name.expr = "expr" # Set expression
```

### NEW: TouchDesigner Modern Python Features

**IMPORTANT: TouchDesigner has introduced new Python libraries and tools. Reference these when writing Python scripts:**

#### TDI Library (TouchDesigner Interface Library)
- **Documentation:** https://docs.derivative.ca/TDI_Library
- **Purpose:** Modern Python interface for TouchDesigner with improved type hints and IDE support
- **When to use:** For new Python scripts that benefit from better autocomplete and type checking
- **Key features:** Better integration with modern Python development tools

#### Thread Manager
- **Documentation:** https://docs.derivative.ca/Thread_Manager
- **Community Post:** https://derivative.ca/community-post/enhancing-your-python-toolbox-touchdesigner%E2%80%99s-thread-manager/72022
- **Purpose:** Simplified multi-threading in TouchDesigner
- **When to use:** For background tasks, parallel processing, and non-blocking operations
- **Key features:** Easy thread management, callbacks, and synchronization

#### Python Environment Manager (tdPyEnvManager)
- **Documentation:** https://docs.derivative.ca/Palette:tdPyEnvManager
- **Community Post:** https://derivative.ca/community-post/introducing-touchdesigner-python-environment-manager-tdpyenvmanager/72024
- **Purpose:** Manage Python packages and virtual environments within TouchDesigner
- **When to use:** For installing and managing external Python dependencies
- **Key features:** Package management, virtual environments, dependency isolation

**Best Practice:** When writing new Python code, check if these modern tools can simplify your implementation before using traditional approaches.

## Project Context

This is a **HydraToTD** project that bridges Hydra visual synthesis with TouchDesigner. Key components:

### Project Structure:
- Main TD file: `hydraToTD.41.toe`
- Python scripts in `/scripts/`
- HTML/JS integration in `/html/`
- Preset system in `/presets/`
- Component library in `/components/`

### Key Systems (Reference Architecture.md for details):
1. **Hydra-to-TouchDesigner translation**
2. **Preset management system**
3. **Multi-output rendering**
4. **Real-time parameter mapping**

### Project Documentation Files:
- **Architecture.md:** System modules, phase buildout, technical implementation notes
- **Implementation Blueprint.md:** Step-by-step build guide, code examples, testing procedures
- **Technical Stack.md:** Performance specs, dependencies, optimization strategies, file structure
- **ProjectDescription.md:** Core features overview, workflow, build priorities

### Inspecting Project Structure

**When you need to understand the current project structure**, run this inspection script:

```python
exec(open(r'scripts/inspect_project_structure.py').read())
```

**Output:** This script saves the complete project structure to `scripts/project_structure.txt` (not printed to textport to avoid overflow).

The output file contains:
- All operators in the project hierarchy
- Operator names and types
- Tree structure with indicators ([COMP], [TOP], [CHOP], [SOP], [DAT], [MAT])
- Up to 3 levels deep by default
- Sorted alphabetically for easy navigation

**After running the script:**
1. The textport will show: "Project structure saved to: scripts/project_structure.txt"
2. Open `scripts/project_structure.txt` to view the complete structure
3. The user can then share relevant sections with you

**Use this script:**
- When you need to locate specific operators
- Before making changes to understand what exists
- To verify the structure after creating new components
- When the user asks "where is X" or "what operators do we have"
- **IMPORTANT:** Always ask the user to run this script and share the output file contents when you need to understand the project structure

## Response Protocol

### Before Every Response:
1. ✅ Read this CLAUDE.md file completely
2. ✅ Check TouchDesigner docs for parameter accuracy
3. ✅ Verify syntax against official API documentation
4. ✅ Check AxiDraw Python API docs if pen plotting is involved
5. ✅ Review relevant project documentation files:
   - Architecture.md for system design context
   - Implementation Blueprint.md for build procedures
   - Technical Stack.md for performance/dependency info
   - ProjectDescription.md for feature requirements

### When Writing TouchDesigner Code:
- Use exact parameter names from documentation
- Include proper error handling
- Follow TouchDesigner Python conventions
- Reference appropriate operator classes
- Validate callback signatures against docs

## Collaborative Workflow

### Code Execution Process:
1. **Claude writes** TouchDesigner Python code
2. **User executes** the code within TouchDesigner environment
3. **User provides feedback** with any errors or issues encountered
4. **Claude iterates** based on actual TouchDesigner error messages

### CRITICAL: TouchDesigner Textport Limitations
**IMPORTANT:** The TouchDesigner textport CANNOT execute multi-line code with indentation (for loops, if statements, function definitions, etc.)

**WHY:** TouchDesigner's textport will throw indentation errors when trying to run any code with indented blocks. This is a limitation of how the textport processes input.

**ALWAYS provide code for textport execution in this format ONLY:**
```python
exec(open(r'scripts/script_name.py').read())
```

**NEVER provide:**
- Multi-line code with indentation (will cause indentation errors)
- Code using semicolons to chain statements
- List comprehensions as "one-liners"
- Complex inline ternary expressions

**If the user needs to run code:**
1. Create a script file in `/scripts/`
2. Provide the `exec(open(r'scripts/...).read())` command
3. That's it - no other format is acceptable due to textport indentation limitations

**Example - WRONG:**
```python
for i in range(10):
    print(i)
```

**Example - ALSO WRONG:**
```python
[print(i) for i in range(10)]
```

**Example - CORRECT (the ONLY acceptable format):**
```python
exec(open(r'scripts/my_script.py').read())
```

### Error Feedback Handling:
When user reports errors from TouchDesigner execution:

#### Expected Error Types:
- **Parameter name errors**: `AttributeError: 'Par' object has no attribute 'paramname'`
- **Operator reference errors**: `AttributeError: op 'operatorname' not found`
- **Syntax errors**: Python syntax issues in TouchDesigner context
- **Runtime errors**: Execution errors during callback or script execution
- **Type errors**: Incorrect parameter types or value ranges

#### Response to Error Feedback:
1. **Analyze the exact error message** provided by user
2. **Re-check TouchDesigner documentation** for correct parameter names
3. **Identify the root cause** (parameter name, operator reference, syntax, etc.)
4. **Provide corrected code** with explanation of the fix
5. **Include debugging suggestions** if applicable

### Best Practices for Code Delivery:
- Provide **complete, runnable code blocks**
- Include **comments explaining TouchDesigner-specific syntax**
- Add **error checking** where appropriate
- Suggest **testing steps** for the user to validate
- Be prepared for **multiple iterations** based on execution feedback

### Documentation Lookup Process:
1. Identify the TouchDesigner operator type (TOP, CHOP, SOP, etc.)
2. Look up the specific operator in the UserGuide
3. Verify parameter names and types
4. Check Python API documentation for method signatures
5. Confirm callback patterns and arguments

## Common TouchDesigner Pitfalls to Avoid:
- Guessing parameter names instead of looking them up
- Using incorrect callback signatures
- Mixing up operator reference syntax
- Assuming parameter types without verification
- Not handling TouchDesigner's frame-based execution model
- **Pulse button callbacks**: Pulse parameters trigger `onPulse(par)`, NOT `onValueChange(par, prev)`
- **Parameter Execute DAT operator reference**: The `ops` parameter requires proper relative (`..`) or absolute (`/project1/comp`) paths, NOT just `.`
- **Falsy parameter values**: When checking if a parameter was created, use `if par is not None:` instead of `if par:` because values like `0`, `""`, `False` are falsy in Python
- **Node sizing**: Use `op.nodeWidth` and `op.nodeHeight` to set node size, NOT `op.par.w` or `op.par.h` (those don't exist)

## Project-Specific Notes:
- This project uses extensive Python callbacks
- Real-time parameter updates are critical
- Multiple output systems require careful operator management
- Preset system needs robust parameter serialization
- **AxiDraw Integration:** Using AxiDraw V3 with brushless servo for SVG plotting
  - Reference pyaxidraw API for pen control parameters
  - Consider brushless servo timing and acceleration settings
  - SVG optimization required for plotting performance

## Component/Module Template System

**CRITICAL: When creating new components or modules, ALWAYS use the BASIC_BLOCK template as the foundation.**

### Using the BASIC_BLOCK Template

The BASIC_BLOCK template is a standardized UI component that provides:
- Custom parameter pages (CUSTOM, BASIC BLOCK, TEXT, ICON, About)
- GLSL shader for rounded rectangles with borders
- Text display and icon support with Material Design Icons
- Proper callbacks and parameter automation
- Consistent styling and behavior across all modules

### How to Create a New Component/Module

**Step 1: Create the BASIC_BLOCK template**

Run this in TouchDesigner textport:
```python
exec(open(r'scripts/load_create_basic_block_template.py').read())
```

This will create a new BASIC_BLOCK at `/project1/BASIC_BLOCK` (or replace existing if `replace_existing=True`).

**Step 2: Customize the template for your module**

After creating the BASIC_BLOCK template, you can:
1. Rename it to your module name (e.g., `ParameterCopier`, `SVGExporter`, etc.)
2. Add your module-specific functionality inside the component
3. Add additional custom parameters as needed
4. Modify the icon and text to reflect the module's purpose

**Step 3: Set the icon and text**

- **Icon**: Set the `Iconhex` parameter to the Material Design Icons hex code (e.g., `F1064`)
- **Text**: Set the `Text` parameter to describe the module
- **Colors**: Customize `Bgcolor`, `Bordercolor`, `Fontcolor` as needed

### Programmatic Template Creation

If you need to create multiple components programmatically, use the `create_basic_block()` function:

```python
# Import the template creation function
exec(open(r'scripts/create_basic_block_template.py').read())

# Create a component with custom name
my_component = create_basic_block(parent_comp=op('/project1'), name='MyModule')

# Customize the component
my_component.par.Iconhex = 'F02D2'  # Set icon
my_component.par.Text = 'My Module'  # Set text
```

### Template Script Location

The template creation script is located at:
- **Main script**: `scripts/create_basic_block_template.py`
- **UTF-8 loader**: `scripts/load_create_basic_block_template.py` (use this for textport execution)

### Why Use the Template?

1. **Consistency**: All components have the same look and feel
2. **Maintainability**: Changes to the template affect all future components
3. **Functionality**: Built-in text editing, icon support, and parameter automation
4. **Professional appearance**: Rounded corners, borders, and proper styling
5. **Time savings**: No need to recreate the same UI elements for each module

**IMPORTANT: Do NOT create bare glslCOMP, containerCOMP, or baseCOMP components for UI modules. ALWAYS start with the BASIC_BLOCK template.**

---

**Remember: Accuracy over speed. Always verify TouchDesigner syntax and parameter names in the official documentation before providing code examples or solutions.**

---

# P3A TouchDesigner Integration Project

## Project Overview

**NOTE**: This repository contains TWO separate TouchDesigner projects:
1. **HydraToTD** - The original project documented above (Hydra visual synthesis integration)
2. **P3A Display Integration** - NEW project for sending TouchDesigner renders to P3A pixel art display

This section documents the **P3A Display Integration** project.

## What is P3A?

P3A (Pixel Pea) is a WiFi-enabled ESP32-P4 based pixel art display device:
- **Hardware**: ESP32-P4 dual-core, 720×720 IPS touchscreen, microSD storage
- **Display**: 4" square MIPI-DSI display with touch input
- **Firmware**: ESP-IDF v5.5.x based animation player
- **Purpose**: Display animated pixel art from SD card or network sources

**Project Goal**: Enable TouchDesigner to send rendered images to P3A display over WiFi in real-time.

## Implementation Status (As of 2025-11-14)

### ✅ CODE COMPLETE: Option 1 - HTTP POST Upload

**Current Phase**: Ready to build firmware and flash to P3A hardware

**Code Status**: All implementation complete, awaiting hardware testing
- ESP32 endpoint implemented ✅
- TouchDesigner scripts implemented ✅
- Testing infrastructure ready ✅
- Documentation complete ✅

**Next Step**: Build ESP-IDF firmware and flash to P3A device (see build instructions below)

**Performance (Estimated)**: 2-7 fps (JPEG), 130-430ms latency

**ESP32 Side** (`components/http_api/http_api.c`):
- Added `POST /upload/image` endpoint
- Accepts raw binary image data (Content-Type: image/png, image/jpeg, etc.)
- Chunked upload (4KB chunks, up to 5MB max)
- Atomic file writes to `/sdcard/animations/td_live.{ext}`
- Immediately calls `animation_player_load_asset()` for display
- Returns JSON response with upload status

**TouchDesigner Side** (`touchdesigner/scripts/`):
- `build_p3a_network.py` - Programmatic network builder
  - Creates Timer CHOP and Execute DAT automatically
  - Embeds upload logic in Execute DAT
  - Fully configurable (TOP name, IP, format, quality, upload rate)
- `p3a_upload_http.py` - Manual upload script for testing
- `README.md` - Complete user documentation

**Testing** (`test_upload.sh`):
- Bash script for ESP32 endpoint validation via curl

### ⏳ PENDING: Option 2 - Direct Memory Display

**Goal**: Bypass SD card for 2-3× speed improvement (10-25 fps)

**Required**:
- New endpoint: `POST /display/raw`
- New API: `animation_player_load_from_memory(data, size, type)`
- RAM buffer management

**Status**: Planned, not started. Awaiting Option 1 hardware validation.

### ⏳ PENDING: Option 3 - WebSocket Streaming

**Goal**: True real-time streaming (20-50 fps) with bi-directional communication

**Required**:
- WebSocket server integration
- Binary frame protocol (custom header + JPEG data)
- Ring buffer (3-frame depth)
- JPEG-only fast path with ESP32-P4 hardware decoder
- TouchDesigner WebSocket DAT integration

**Status**: Planned, not started. Only needed if high frame rates required.

## Key Learnings for Future Sessions

### P3A Architecture Insights

1. **Animation Player is Memory-Based**
   - Files loaded entirely into RAM via `load_animation_file_from_sd()`
   - Decoder interface accepts `const uint8_t *data, size_t size`
   - **Implication**: Option 2 just needs to skip file I/O, can reuse decoder pipeline

2. **Animation Directory**: `/sdcard/animations/`
   - Player scans this directory on startup
   - Our `td_live.{ext}` file appears in rotation automatically
   - Configured via Kconfig: `CONFIG_P3A_SD_ANIMATIONS_DIR`

3. **HTTP Server Has Room for Growth**
   - `max_uri_handlers = 12`, currently using 10 (2 slots remaining)
   - Can add `/display/raw` for Option 2
   - WebSocket uses same HTTP server (ESP-IDF built-in support)

4. **Display Pipeline**:
   ```
   File/Memory → Decoder → Native Frame → Upscale → LCD Buffer → DMA → Display
   ```
   - Double-buffered rendering
   - Hardware upscaling if source < 720×720

### TouchDesigner Integration Patterns

1. **TOP Pixel Export**:
   ```python
   img_array = top_op.numpyArray(delayed=False)
   # Returns shape (height, width, channels) as float32 (0.0-1.0)
   # Bottom-left origin - flip with np.flipud()
   ```

2. **Network Builder Pattern**:
   ```python
   timer_chop = parent_comp.create(timerCHOP, 'name')
   execute_dat = parent_comp.create(executeDAT, 'name')
   timer_chop.par.timeout = 2  # 2 Hz upload rate
   execute_dat.par.chopexec0 = timer_chop  # Wire reference
   execute_dat.text = '''def onValueChange(...): ...'''  # Embed code
   ```
   - More reliable than manual .toe files
   - User can customize after generation

3. **Python Dependencies**: `requests`, `pillow`, `numpy`
   - Install via tdPyEnvManager (Palette component)
   - Required for image encoding and HTTP POST

### Known Issues / Edge Cases

1. **Multipart vs Raw Binary**
   - TouchDesigner `requests.post(files=...)` sends multipart/form-data
   - Current ESP32 handler expects raw binary
   - **Status**: Works because Python `requests` library encodes properly
   - **Watch**: May need multipart parser if issues arise

2. **Thread Safety**
   - We call `animation_player_load_asset()` from HTTP handler thread
   - Appears safe (no crashes in code review)
   - **Watch**: Possible race condition if player is mid-decode

3. **Memory Fragmentation**
   - Each upload allocates 4KB buffer (freed after use)
   - Large files loaded by animation player (100KB-2MB)
   - **Monitor**: Check `/status` endpoint for `heap_free`

## File Locations

```
tdESP32display/
├── components/http_api/http_api.c         [MODIFIED] Option 1 endpoint
├── touchdesigner/                         [NEW]
│   ├── README.md                          User guide
│   └── scripts/
│       ├── build_p3a_network.py          Network builder
│       └── p3a_upload_http.py            Manual upload
├── test_upload.sh                         [NEW] Testing script
├── P3A_STATUS.md                          [NEW] Detailed status doc
├── TOUCHDESIGNER_INTEGRATION.md           [NEW] Planning doc (all 3 options)
└── CLAUDE.md                              [THIS FILE]
```

## Quick Commands for Future Sessions

### Test ESP32 Endpoint
```bash
./test_upload.sh p3a.local test_image.jpg
```

### Setup TouchDesigner Network
```python
# In TouchDesigner textport:
exec(open(r'touchdesigner/scripts/build_p3a_network.py').read())
op('p3a_upload_timer').par.start.pulse()  # Start uploading
```

### Check Device Status
```bash
curl http://p3a.local/status | python3 -m json.tool
```

## Build & Flash Instructions (CURRENT STEP)

### Step 1: Install ESP-IDF v5.5.x

```bash
# Clone ESP-IDF
cd ~/
git clone -b v5.5 --recursive https://github.com/espressif/esp-idf.git esp-idf-v5.5
cd ~/esp-idf-v5.5
./install.sh esp32p4

# Source environment (add to ~/.bashrc for persistence)
. ~/esp-idf-v5.5/export.sh
```

### Step 2: Build Firmware

```bash
cd /home/user/tdESP32display

# Set target
idf.py set-target esp32p4

# Configure (optional)
idf.py menuconfig

# Build (takes 5-15 minutes first time)
idf.py build
```

### Step 3: Flash to P3A

```bash
# Connect P3A via USB, then flash and monitor
idf.py -p /dev/ttyUSB0 flash monitor

# Exit monitor with Ctrl+]
```

### Step 4: Configure WiFi

- Connect to `p3a-setup` WiFi network
- Browse to `http://192.168.4.1`
- Enter your WiFi credentials

### Step 5: Test Upload Endpoint

```bash
./test_upload.sh p3a.local
```

See detailed build guide in P3A_STATUS.md for troubleshooting.

## Next Actions After Firmware Flash

1. **Test Option 1 on Real Hardware**
   - Run `test_upload.sh` to validate ESP32 endpoint
   - Run TouchDesigner network builder and test uploads
   - Measure actual performance (latency, frame rate)

2. **Debug Any Issues**
   - SD card path verification
   - WiFi connectivity issues
   - Multipart parsing if needed
   - Memory usage monitoring

3. **Implement Option 2** (if faster uploads needed)
   - Add `POST /display/raw` endpoint
   - Add `animation_player_load_from_memory()` function
   - Expected: 2-3× speed improvement

4. **Implement Option 3** (if real-time streaming needed)
   - Only if >20 fps required
   - Significant effort (1-2 weeks)

## Important Context Files

- **P3A_STATUS.md** - Comprehensive status document with detailed learnings
- **TOUCHDESIGNER_INTEGRATION.md** - Full planning doc for all 3 options
- **touchdesigner/README.md** - User-facing quick start guide

## Git Branch

**Current Branch**: `claude/repo-exploration-011CV5aUjwhXLuEnDAMLoWfk`

**Key Commits**:
- `660999a` - P3A status document
- `c79540e` - Option 1 implementation (ESP32 + TouchDesigner)
- `2b019a8` - TouchDesigner integration planning doc updates
- `ac231b6` - Initial TouchDesigner integration planning doc

---

**For Future Claude**: When resuming P3A work, read `P3A_STATUS.md` for detailed context. Option 1 code is complete and ready for firmware build/flash. See "Build & Flash Instructions" section above for next steps.
