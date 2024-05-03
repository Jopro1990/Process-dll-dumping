# Manual DLL Dumping using Process Hacker and PE Unmapper

- This approach involves manually inspecting the target process's memory and identifying sections that contain injected code. It requires tools like Process Hacker and PE Unmapper.
- Tools Required: https://processhacker.sourceforge.io/; https://github.com/hasherezade/pe_unmapper

## Step 1: Identify the Target Process
1) Open Process Hacker and find the process where the DLL is injected. This could be a legitimate process that exhibits unusual behavior or high CPU usage.
2) Double-click on the process to view its properties.

## Step 2: Locate Suspicious Memory Sections
1) Go to the "Memory" tab in Process Hacker.
2) Look for memory sections with Read-Write-Execute (RWX) permissions, which are often used for injected code. Sometimes, you might find Read-Execute (RX) sections, but RWX is more common for injected DLLs.
3) Double-click on each RWX section to inspect its contents. Look for telltale signs of a DLL, such as the "MZ" header or recognizable function prologues (like "55 8b"). If you find such a section, note its start address and size.

## Step 3: Dump the DLL to Disk
1) Once you find a memory section that resembles a DLL, right-click on it and select "Save Memory" to dump it to a file.
2) Note the start address of this memory section, as it will be needed for later steps.

## Step 4: Convert to a Raw PE File
1) Open PE Unmapper and use the following command to convert the memory dump to a raw Portable Executable (PE) format:
   pe_unmapper DUMPED_FILE_NAME START_ADDRESS
   Replace DUMPED_FILE_NAME with the file name of your memory dump, and START_ADDRESS with the start address noted earlier.
2) This step reconstructs the raw PE structure of the DLL, which can then be analyzed further or loaded into a debugger for examination.

## Step 5: Validate the Dumped DLL
1) Open the reconstructed PE file in a tool like PEview, CFF Explorer, or IDA Pro to verify its structure and ensure it's a valid DLL.
2) Examine the imports, exports, and other sections to determine if it's malicious or part of the original process.
