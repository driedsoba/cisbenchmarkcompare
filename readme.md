# 🔒 CIS Benchmark Comparison Tool

A comprehensive multi-platform tool for comparing CIS (Center for Internet Security) benchmarks across different operating systems and versions.

## 🚀 Features

- **🪟 Windows Server 2022** - Compare CIS controls with company standards or other Windows versions
- **🐧 RHEL 8** - Map CIS controls against company policies, RHEL 9, or other Linux distributions
- **🔍 Smart Comparison Engine** - Intelligent similarity matching with adjustable thresholds
- **📊 Detailed Results** - Categorized matches (exact/similar/platform-specific)
- **📁 Export Options** - CSV and JSON export for documentation and integration
- **📝 Sample Data** - Real CIS benchmark controls included for testing

## 🌐 Live Demo

[**Try the tool live**]([https://your-vercel-url.vercel.app](https://cisbenchmarkcompare.vercel.app/))

## 🎯 Use Cases

- **Security Teams**: Map CIS controls across different platforms
- **Compliance**: Document control coverage and identify gaps
- **Migration Projects**: Compare RHEL 8→9, Windows 2019→2022
- **Auditing**: Validate against company security standards
- **Vendor Assessment**: Compare different OS security baselines

## 📖 How to Use

### 1. Select Platform
Choose between Windows Server 2022 or RHEL 8 comparison pages.

### 2. Input Data
**Format**: Each line should be: `Control_ID    Title` (tab separated or multiple spaces)

**Windows Example:**
```
1.1.1    Ensure 'Enforce password history' is set to '24 or more passwords'
1.1.2    Ensure 'Maximum password age' is set to '365 or fewer days, but not 0'
```

**RHEL Example:**
```
1.1.1.1    Ensure mounting of cramfs filesystems is disabled
1.1.1.2    Ensure mounting of freevxfs filesystems is disabled
```

### 3. Load Sample Data
Click **"📝 Load Sample Data"** to see real CIS benchmark controls:
- **Windows**: 25+ controls covering password policies, firewall, security options
- **RHEL**: 100+ controls covering filesystem, network, audit, SSH, permissions

### 4. Adjust Similarity Threshold
Use the slider to control matching sensitivity (see detailed explanation below).

### 5. Compare & Export
Run comparison and export results to CSV or JSON for documentation.

## 🎛️ How the Similarity Threshold Works

The similarity threshold determines how "similar" two controls need to be to be considered a match.

### Algorithm Details

The tool uses **Jaccard similarity** based on word matching:

1. **Normalize text**: Remove punctuation, convert to lowercase
2. **Split into words**: "Ensure SSH root login is disabled" → ["ensure", "ssh", "root", "login", "is", "disabled"]
3. **Compare word sets**: Count shared words vs total unique words
4. **Calculate percentage**: (Shared words) / (Total unique words) = Similarity score

### Threshold Examples

**80% Threshold (Recommended):**
```
RHEL 8: "Ensure SSH root login is disabled"
RHEL 9: "Ensure SSH root login is prohibited"

Shared words: ["ensure", "ssh", "root", "login", "is"]
Total words: ["ensure", "ssh", "root", "login", "is", "disabled", "prohibited"] 
Similarity: 5/7 = 71% → NO MATCH (below 80%)
```

**70% Threshold (More Permissive):**
```
Same example above:
71% → MATCH (above 70%)
```

### Real-World Examples

**Example 1: Minor Wording Changes**
```
Left:  "Ensure 'Maximum password age' is set to '365 or fewer days'"
Right: "Ensure 'Maximum password age' is configured to '365 days or less'"

Words in common: ["ensure", "maximum", "password", "age", "is", "to", "365", "days"]
Total unique words: 14
Similarity: 8/14 = 57%

30% threshold: ✅ MATCH
50% threshold: ✅ MATCH  
70% threshold: ❌ NO MATCH
80% threshold: ❌ NO MATCH
```

**Example 2: Very Similar Controls**
```
Left:  "Ensure SSH root login is disabled"
Right: "Ensure SSH root access is disabled"

Words in common: ["ensure", "ssh", "root", "is", "disabled"]
Total unique words: 7
Similarity: 5/7 = 71%

50% threshold: ✅ MATCH
70% threshold: ✅ MATCH
80% threshold: ❌ NO MATCH
90% threshold: ❌ NO MATCH
```

### Threshold Recommendations

| **Threshold** | **Best For** | **Results** |
|---------------|--------------|-------------|
| **30-50%** | Broad mapping, different vendors | Many matches, some false positives |
| **60-70%** | Cross-version comparison (RHEL 8→9) | Good balance, catches rephrasing |
| **80%** | **Recommended default** | High confidence matches |
| **90-100%** | Strict matching, same vendor | Only very similar controls |

### Practical Impact

**Low Threshold (30-50%):**
- ✅ Catches more potential matches
- ❌ May include false positives
- 💡 Good for initial exploration

**Medium Threshold (70-80%):**
- ✅ Good balance of accuracy and coverage
- ✅ Catches legitimate rephrasing
- 💡 **Best for most use cases**

**High Threshold (90-100%):**
- ✅ Only high-confidence matches
- ❌ May miss valid similar controls
- 💡 Good for final validation

## 📊 Results Categories

The tool categorizes comparison results into four types:

### ✅ Exact Matches
Controls with identical titles (regardless of different IDs)
- **Use**: Direct mapping between versions
- **Confidence**: 100%

### 🔍 Similar Matches  
Controls that meet the similarity threshold
- **Use**: Likely equivalent controls with different wording
- **Confidence**: Based on similarity percentage

### ❌ Left Only
Controls that exist only in the first dataset
- **Use**: Identify deprecated or removed controls
- **Action**: Review if still needed

### ➕ Right Only
Controls that exist only in the second dataset
- **Use**: Identify new or added controls
- **Action**: Consider implementation

## 📁 Export Formats

### CSV Export
Structured data for spreadsheet analysis:
```csv
Type,Left_ID,Left_Title,Right_ID,Right_Title,Similarity
Exact,"1.1.1","Ensure password history","1.1.1","Ensure password history",100%
Similar,"1.1.2","Maximum password age","1.1.2","Password age limit",85%
```

### JSON Export
Machine-readable format for automation and integration:
```json
{
  "exactMatches": [...],
  "similarMatches": [...],
  "leftOnly": [...],
  "rightOnly": [...]
}
```

## 🛠️ Technical Details

- **Frontend**: Pure HTML5, CSS3, JavaScript (no dependencies)
- **Algorithm**: Jaccard similarity with word tokenization
- **Responsive**: Works on desktop, tablet, and mobile
- **Privacy**: All processing happens client-side
- **Export**: Modern browser download API with fallbacks


### Local Development
```bash
# Clone repository
git clone https://github.com/driedsoba/cisbenchmarkcompare.git
cd cisbenchmarkcompare

# Serve locally (any HTTP server)
python -m http.server 8000
# or
npx serve .

# Open browser
open http://localhost:8000
```

## 📝 Sample Data Sources

The tool includes real CIS benchmark controls:

### Windows Server 2022
- **Source**: CIS Microsoft Windows Server 2022 Benchmark v2.0.0
- **Controls**: Password policies, account lockout, firewall settings, administrative templates
- **Categories**: Account policies, user rights, security options, Windows Firewall

### RHEL 8
- **Source**: CIS Red Hat Enterprise Linux 8 Benchmark v3.0.0
- **Controls**: Filesystem configuration, network parameters, SSH hardening, audit settings
- **Categories**: Initial setup, services, network, logging, access control, system maintenance


## 🔗 Related Resources

- [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks/)
- [CIS Controls](https://www.cisecurity.org/controls/)
- [Windows Server 2022 CIS Benchmark](https://www.cisecurity.org/benchmark/microsoft_windows_server)
- [RHEL 8 CIS Benchmark](https://www.cisecurity.org/benchmark/red_hat_linux)
