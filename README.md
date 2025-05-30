# 🧬 StructFlow Colab Guide   

This notebook walks you through generating CellPACK recipes from sequence and simulation data, predicting structures with AlphaFold, and preparing JSON files for 3D modeling.

---

## ✅ Step 1: Input Protein Sequences  

Mount your **Google Drive** and load a CSV file with protein sequences.  

📁 **Location:** `/MyDrive/CellPACK/Protein_Sequences_Template.csv`  

Each row must contain:
- `id`: unique identifier  
- `sequence`: protein sequence (AAs)

This cell will:
- Mount Google Drive
- Load your sequence CSV
- Prepare job folders for each protein
- Save working copy into Colab

---

## 🔬 Step 1.1: Run AlphaFold Prediction (ColabFold)

Run **AlphaFold2** predictions for all sequences using ColabFold.  

You can customize:
- `template_mode`: `"none"`, `"pdb100"`, or `"custom"`
- `num_relax`: `0`, `1`, or `5` relaxation steps

The model:
- Automatically detects if the sequence is a complex
- Saves predicted `.pdb` files to `/content/proteins`

---

## 📦 Step 2: Connect Input Files for Recipe Generation  

Copy required metadata and simulation files for your specific cell model.

📁 **Expected Google Drive Folder:** `/MyDrive/CellPACK/CellPACK_Input_Files/`

Required files include:
- `protein_data.json`
- `genes_data.json`
- `simulation-1.h5`
- `getSeriesData_monomers1.json`
- `getSeriesData_complexes1.json`
- `WholeCellData.xlsx`
- `ingredient_function.csv`
- `method_selection.csv`
- `compartment_updated.json`
- `updated_function_dictionary.json`
- `all_dict.json`

These will be copied to: `/content/input_files/`

---

## 🧱 Step 2.1: Connect CellPACK Data Assets  

This step pulls data assets used during 3D model construction.  

📁 **Drive Source Folder:** `/MyDrive/CellPACK/CellPACK_Data/`

Copied folders:
- ✅ `lattices/` — required for LatticeNucleoid modeling  
- 🎨 `palettes/` — optional visualization files  
- 💾 `proteins/` — your predicted AlphaFold models (auto-copied)

Output directory: `/content/cellPACK_Data`

---

## 🧪 Step 2.2: Generate CSV Recipes (Auto / Curated)  

Choose how you'd like to generate your recipe files:

```python
recipe_mode = "both"  # Options: "curated", "auto", "both"

# 🔧 Definitions:

"curated" – Generates a recipe using manually curated homology models and experimentally validated PDB structures.

"auto" – Includes only proteins with existing solved homologs, without applying any homology modeling.

"both" – Produces both the curated and automated recipe variants for side-by-side use or comparison.

---

## 🌟 Step 2.3: Customize Frame Numbers (Copy Number Control)

Frame numbers determine which snapshot of the `.h5` simulation is used to calculate molecular copy numbers. This step is essential for producing realistic densities in the final 3D cell model.

In the builder script (`WC-MG-CellPACK-RecipeBuilder-short.py`), locate the following lines:

- `frame_mono = 149`
- `frame_complex = 150`

You can modify these values to target different timepoints in the simulation.

### 🔢 Example Frame Options:
- `frames_m = [150, 1185, 6974]`  ← for monomers
- `frames_c = [146, 1190, 6961]`  ← for complexes

🔀 Change these values to explore how molecular counts vary across different simulation snapshots.

---

## 📄 Step 3: Convert CSV to JSON for CellPACK-GPU

Once you have generated `auto_root.csv` or `curated_root.csv`, the next step is to convert it into a CellPACK-compatible JSON recipe.

📁 These files are saved automatically in your Colab workspace inside the folder:  
`scripts_output/`

🔗 Open the Mesoscope Web Interface:  
👉 https://mesoscope.scripps.edu/beta/#

### 📥 To convert your CSV:

1. Click **"Load"** (top left) and upload your `auto_root.csv` or `curated_root.csv` from the `scripts_output` folder.  
2. Your ingredients will appear in the **Recipe Table** (bottom panel).  
3. Click **"Save"** → **cellPACK-gpu recipe** to export the `.json` file.

💡 This JSON file can now be used with **CellPACK-GPU** or **Simularium** to assemble and visualize your 3D whole-cell model.

---

## 🚫 Step 4: Run CellPACK-GPU Locally (Manual Setup Required)

Due to limited access to internal automation scripts, **this step can no longer be executed directly via the Colab pipeline**.

Instead, users can manually download and run **CellPACK-GPU** using the official release.

---

### 📦 Download CellPACK-GPU

🔗 Download it from the official GitHub release page:  
👉 [MycoplasmaGenitalium v1.0 – CellPACK-GPU](https://github.com/ccsb-scripps/MycoplasmaGenitalium/releases/tag/v1.0)

1. Scroll down to the **Assets** section.
2. Download the `cellPACKGPU.zip` file for your operating system.

---

### 🧰 How to Use It

1. **Extract** the ZIP file to a known location on your computer (e.g., Desktop or Documents).
2. Inside the extracted folder, locate the file named:
   - `cellPACKgpu.exe` (on Windows)
3. **Double-click** to launch the application.

You’ll see the CellPACK-GPU interface, similar to a Unity-based simulation window. This tool allows you to load your `.json` recipe (from Mesoscope) and simulate spatial packing.

---

## 👥 Contributors

**Markus Gavra** and **Evan Bucholski**  
Second-year Software Engineering students at the University of Guelph, with a focus on applying computational tools to real-world scientific challenges.


