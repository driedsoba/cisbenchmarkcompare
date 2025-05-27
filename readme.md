# 🔒 CIS Benchmark Comparison Tool

![Version](https://img.shields.io/badge/version-1.0.0-blue)  
![License](https://img.shields.io/badge/license-MIT-green)

A comprehensive multi-platform tool for comparing CIS (Center for Internet Security) benchmarks across different operating systems and versions.

🌐 **Live Demo**: [cisbenchmarkcompare.vercel.app](https://cisbenchmarkcompare.vercel.app)

## ✨ Features

- 🪟 **Windows Server 2022** benchmark comparison  
- 🐧 **RHEL 8** Linux benchmark comparison  
- 📊 **Smart matching algorithm** with adjustable similarity threshold  
- 📁 **Export to CSV/JSON** for documentation  
- 🎯 **ID overlap analysis** to identify control gaps  
- 📱 **Responsive design** for all devices  

## ⚙️ How the Matching Algorithm Works

1. **Parsing Controls**  
   - Input text is split line-by-line and each line is divided into `Control_ID` and `Title` by tabs or multiple spaces.  
   - Results in two arrays of control objects: `{ id, title }` for left and right datasets.

2. **Exact Matching**  
   - Find controls with identical IDs **and** identical titles (case-insensitive).  
   - Mark these as **Exact Matches** (100% similarity).

3. **Same ID, Different Content**  
   - For controls sharing the same ID but with different titles:  
     - Compute a **Jaccard similarity** on the normalized words of each title.  
     - Extract added/removed words to highlight differences.  
     - Mark these as **Same ID, Different Content** with their similarity score.

4. **Fuzzy Similarity Matching**  
   - For remaining unmatched controls, compare titles across datasets:  
     - Normalize text (remove punctuation, lowercase, split into word sets).  
     - Compute Jaccard similarity: `|intersection| / |union|`.  
     - If similarity ≥ threshold (adjustable slider), mark as **Similar Matches**.

5. **Left-Only & Right-Only Controls**  
   - Any controls on the left side without a match become **Left-Only**.  
   - Any controls on the right side not used in matches become **Right-Only**.

6. **ID Overlap Analysis**  
   - Calculate total unique IDs across both sets.  
   - Compute the overlap percentage of shared IDs.  
   - Display summary statistics in the ID Analysis section.

This step-by-step approach ensures accurate detection of exact, modified, and semantically similar controls, while also identifying gaps in coverage.

## 🚀 Quick Start

### Option 1: Use Online  
Visit [cisbenchmarkcompare.vercel.app](https://cisbenchmarkcompare.vercel.app)

### Option 2: Deploy Your Own
1. Fork this repository  
2. Deploy to Vercel: [![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/driedsoba/cisbenchmarkcompare)  
3. Your instance will be live in minutes!  

### Option 3: Run Locally
```bash
git clone https://github.com/driedsoba/cisbenchmarkcompare.git
cd cisbenchmarkcompare
# Open index.html in your browser or use a local server
python -m http.server 8000
# Visit http://localhost:8000
