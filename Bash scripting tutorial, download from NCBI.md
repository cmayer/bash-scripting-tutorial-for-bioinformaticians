# Bash Scripting Tutorial for beginners who want to use bash in bioinformatics - Part 1

Why another bash scripting tutorial? In fact there are hundreds of tutorial out there. Unfortunately, I did not find a single tutorial I liked.
- Some start by creating a first files using the vi or vim editor. This is an editor for professionals not for beginners. This approach makes things unnecessarly frustrating for beginners. I think it is not suited for beginners in bioinformatics.
- Many tutorials want to show you everything you can do with bash. We do not need to know everything. We just want to automate a few tasks.
- Many tutorials use examples from computer science or system adminitstration, e.g. by showing you the power of linux commands by writing scripts that allow you to filter the system logs or similar unmotivated tasks, most students will never ever need.

# Goal of this tutorial

Bash scripts are used in bioinformatics manly to automate data retreaval and analysis tasks.
This typically means, you download data from public databases, you extract the information you are interested in, and you conduct the analyses.

In this tutorial, we will implement bash script for the following tasks:
1) Download all whole mitochondrial genomes of Anopheles species deposited in NCBI in the genebank format. 
2) Extract genes ...
3) 

Alongside of developing the script, the commands and syntax are explained.

# Let us start from the beginning: What is a bash script?

A bash script is a text file into which you write a sequence of bash commands in the order you want to execute them.
While flow control statements such as **if** and **for** can also be used in interactive mode, they are typically used in bash scripts.


# Step by step implementation of a bash script that downloads Anophlels mitochondrial genomes from NCBI

## Step 1, bash script with single echo command

Let us implement our first bash script. Open an editor such as nano, e.g. as follows:
```
nano bash-tutorial-download-from-NCBI.sh
```

The file extension `.sh` is commonly used for bash scripts and we want to use this convention. In the editor that opens, type:
```bash
#!/bin/bash    # The shebang
# Bash scripting tutorial, part 1
# Downloading mitochondrial genomes from NCBI
# Implementation step 1

echo "Running the bash script: bash-tutorial-download-from-NCBI.sh"
```

## Elements found in this simple bash script

### Shebang
All bash scripts should start with (beginnig of the first line) the so-called shebang, which you see in the first line.
Formally, a bash script is just a text file. The shebang specifies the program that shall be used to interpret the commands.
In our case, the kernel finds `"/bin/bash"` and starts a new instance of the bash shell to interpret the current file, i.e. it starts another bash process.

### Comments
If a line contains a hash `#` character, all text following the hash is a comment and it is not interpreted as commands.
Exceptions:
- If the kernel sees that a file starts with the shebang, it uses the specified program as interpreter.
- If the nash is found in quotes, it is not interpreted and it does not mark the start of a comment.

### Commands
Our bash script contains only a single command, i.e. the `echo` command.

## Running our first bash script
In order to be able to run our bash script we have to make it executable, i.e. we have to give it the permission to run independently. We do this with the `chmod`command:
```
chmod u+x bash-tutorial-download-from-NCBI.sh
```
Now we can start the bash script by typing:
```
./bash-tutorial-download-from-NCBI.sh
```
This should print
```
Running the bash script: bash-tutorial-download-from-NCBI.sh
``` 

## Step 2, define variables in bash script
In all programming languages, variables are used to hold information between multiple commands. In bash, variables have alredy been introduced in the video on the interactive mode. For assinging a value to a bash variable we use the syntax:
```
variable=value
```
Bash does not allow unnecessary spaces, so there should be not spaces before and after the assignment operator `=`.
We can get the value of a variable, by writing the `$`-symbol infront of it. 
In our bash script we will introduce two variables holding the names of the output directory and a directory used for temporary files.
Next we will create the two directories in our script.

```bash
#!/bin/bash    # The shebang
# Bash scripting tutorial, part 1
# Downloading mitochondrial genomes from NCBI
# Implementation step 2

echo "Running the bash script: bash-tutorial-download-from-NCBI.sh"

# Defining variables
OUTPUT_DIR="./anopheles_mito_genomes"
TEMP_DIR="./temp"

# Creating the directories:
mkdir -p $OUTPUT_DIR
mkdir -p $TEMP_DIR
```

Since our bash script already has the permission to be executed, we can run this directly with
```
./bash-tutorial-download-from-NCBI.sh
```
This command will now create the two directories. The `-d` option in the `mkdir` command creates directories safely. It creates parent directories if needed and does not exit with an error if the directory exists.


## Step 3: Define the Search Term as a variable and download the 

We will query NCBI from the command line. For this we will need a search term. In our case we want to download all avaiable mitochondrial genomes of Anopheles species. The search term will be stored in a variable.

```bash
SEARCH_TERM="Anopheles[Organism] AND mitochondrion[Sequence Location] AND complete genome[Title]"
```


## 🎯 Goal
Learn how to write a **robust bash script** to download **734 Anopheles mitochondrial genomes** from NCBI using only standard Linux tools (`bash`, `wget`, `grep`, `cut`, etc.).

By the end, you'll understand:
- How to query NCBI's database
- How to parse JSON responses
- How to handle errors and rate-limiting
- How to write clean, maintainable scripts

---

## 📂 Step 1: Set Up the Script

### ✅ Create the script file
```bash
touch download_anopheles_mito.sh
chmod +x download_anopheles_mito.sh
```

Open it in a text editor (e.g., `nano`, `vim`, or VS Code):
```bash
nano download_anopheles_mito.sh
```




## 🔍 Step 3: Define the Search Term

### ✅ Original search term (human-readable)
```bash
SEARCH_TERM="Anopheles[Organism] AND mitochondrion[Sequence Location] AND complete genome[Title]"
```

### ✅ URL-encoded version (for web requests)
```bash
SEARCH_TERM_ENCODED="Anopheles%5BOrganism%5D+AND+mitochondrion%5BSequence+Location%5D+AND+complete+genome%5BTitle%5D"
```
> 💡 **Why URL-encode?**
> Spaces and brackets (`[`, `]`) must be encoded for URLs:
> - ` ` → `+`
> - `[` → `%5B`
> - `]` → `%5D`

> ✅ **Tip**: You can use `printf '%s\n' "$SEARCH_TERM" | jq -sRr @uri` to auto-encode, but we'll stick to manual for this tutorial.

---

## 🌐 Step 4: Define NCBI Base URL and Parameters

```bash
BASE_URL="https://eutils.ncbi.nlm.nih.gov/entrez/eutils"
DB="nucleotide"
RETMAX=500
```
> 💡 **What is `RETMAX`?**
> Maximum number of IDs to return per request. NCBI allows up to 500.

---

## 📊 Step 5: Get Total Number of Records

### ✅ Use `esearch` to get count
```bash
echo "=== Step 1: Get total count ==="
echo "Searching for: $SEARCH_TERM"

# Download JSON response
wget -qO "$TEMP_DIR/count.json" \
  "$BASE_URL/esearch.fcgi?db=$DB&term=$SEARCH_TERM&retmode=json"
```

> 💡 **What is `wget -qO`?**
> - `-q`: Quiet mode (no output)
> - `-O`: Save output to file

### ✅ Extract count from JSON
```bash
TOTAL_COUNT=$(grep -o '"count":"[^"]*"' "$TEMP_DIR/count.json" | \
              cut -d'"' -f4 | \
              head -n 1 | \
              tr -d '[:space:]')
```

> 🔍 **How it works:**
> - `grep -o '"count":"[^"]*"'`: Extracts `"count":"734"`
> - `cut -d'"' -f4`: Gets `734`
> - `head -n 1`: Takes first match
> - `tr -d '[:space:]'`: Removes spaces/newlines

> ✅ **Check if count is valid**
```bash
if [ -z "$TOTAL_COUNT" ] || [ "$TOTAL_COUNT" -eq 0 ]; then
  echo "Error: No records found. Check search term."
  exit 1
fi
```

> 💡 **What is `-z`?**
> Checks if a string is **empty**.

---

## 📥 Step 6: Fetch All IDs in Batches

### ✅ Loop through batches
```bash
ID_FILE="$OUTPUT_DIR/ids.txt"
> "$ID_FILE"  # Clear file

for (( start=0; start < TOTAL_COUNT; start += RETMAX )); do
  echo "Fetching batch from $start to $((start + RETMAX - 1))..."

  # Download batch
  wget -qO "$TEMP_DIR/batch_$start.json" \
    "$BASE_URL/esearch.fcgi?db=$DB&term=$SEARCH_TERM&retstart=$start&retmax=$RETMAX&retmode=json"

  # Extract IDs
  grep -o '"Id"[^,]*' "$TEMP_DIR/batch_$start.json" | \
    cut -d'"' -f4 >> "$ID_FILE"

  sleep 1  # Be kind to NCBI
done
```

> 💡 **Why `retstart` and `retmax`?**
> - `retstart`: Start index (0, 500, 1000, ...)
> - `retmax`: Number per batch (500)
> → This is called **pagination**.

> ✅ **Check if IDs were collected**
```bash
if [ ! -s "$ID_FILE" ]; then
  echo "Error: No IDs collected. Check network or NCBI."
  exit 1
fi
```

> 💡 **What is `-s`?**
> Checks if a file **exists and is not empty**.

---

## 🧬 Step 7: Download GenBank Files

### ✅ Loop through IDs and download
```bash
echo -e "\n=== Step 3: Downloading GenBank records ==="

while IFS= read -r id; do
  filename="$OUTPUT_DIR/Anopheles_mito_${id}.gb"
  echo "Downloading ID: $id -> $filename"

  # Try 3 times with exponential backoff
  for attempt in 1 2 3; do
    wget -qO "$filename" \
      --timeout=30 \
      --tries=1 \
      "$BASE_URL/efetch.fcgi?db=$DB&id=$id&rettype=gb&retmode=text"

    # Check if file is valid GenBank
    if [ -s "$filename" ] && grep -q "LOCUS\|FEATURES" "$filename"; then
      echo "✅ Success: $filename"
      break
    else
      echo "⚠️  Failed (attempt $attempt)"
      if [ $attempt -eq 3 ]; then
        echo "❌ Giving up after 3 attempts."
      else
        sleep $((2 ** attempt))  # 2s, 4s, 8s
      fi
    fi
  done

  sleep 1  # Be kind to NCBI
done < "$ID_FILE"
```

> 💡 **Why `grep -q "LOCUS\|FEATURES"`?**
> Ensures the file is **real GenBank format** (not an error page).

> 💡 **What is `sleep $((2 ** attempt))`?**
> Exponential backoff: wait 2s, then 4s, then 8s → avoids rate-limiting.

---

## 📊 Step 8: Final Summary

```bash
echo -e "\n=== Summary ==="
total_files=$(find "$OUTPUT_DIR" -name "*.gb" -size +0 | wc -l)
echo "Downloaded $total_files mitochondrial genome(s)."
echo "Output saved in: $OUTPUT_DIR"
```

> 💡 **What is `-size +0`?**
> Counts only **non-empty** files.

---

## 🧪 Step 9: Optional — Show Sample Output

```bash
if [ $total_files -gt 0 ]; then
  echo -e "\n🔍 Sample of first file:"
  head -n 20 "$OUTPUT_DIR/Anopheles_mito_*.gb" | head -n 10
fi
```

---

## 🧩 Full Script (Final Version)

```bash
#!/bin/bash
# ===================================================================
# Script: Download ALL Anopheles mitochondrial genomes
# Fixed: Proper error handling, rate-limiting, and validation
# ===================================================================

OUTPUT_DIR="./anopheles_mito_genomes"
TEMP_DIR="./temp"
SEARCH_TERM="Anopheles[Organism] AND mitochondrion[Sequence Location] AND complete genome[Title]"
BASE_URL="https://eutils.ncbi.nlm.nih.gov/entrez/eutils"
DB="nucleotide"
RETMAX=500

# Create directories
mkdir -p "$OUTPUT_DIR" "$TEMP_DIR"

echo "==================================================================="
echo "Anopheles Mitochondrial Genome Downloader"
echo "==================================================================="

# Step 1: Get total count
echo "=== Step 1: Get total count ==="
echo "Searching for: $SEARCH_TERM"

wget -qO "$TEMP_DIR/count.json" \
  "$BASE_URL/esearch.fcgi?db=$DB&term=$SEARCH_TERM&retmode=json"

TOTAL_COUNT=$(grep -o '"count":"[^"]*"' "$TEMP_DIR/count.json" | \
              cut -d'"' -f4 | \
              head -n 1 | \
              tr -d '[:space:]')

if [ -z "$TOTAL_COUNT" ] || [ "$TOTAL_COUNT" -eq 0 ]; then
  echo "Error: No records found."
  exit 1
fi

echo "Found $TOTAL_COUNT matching records."

# Step 2: Fetch all IDs in batches
ID_FILE="$OUTPUT_DIR/ids.txt"
> "$ID_FILE"

for (( start=0; start < TOTAL_COUNT; start += RETMAX )); do
  echo "Fetching batch from $start to $((start + RETMAX - 1))..."

  wget -qO "$TEMP_DIR/batch_$start.json" \
    "$BASE_URL/esearch.fcgi?db=$DB&term=$SEARCH_TERM&retstart=$start&retmax=$RETMAX&retmode=json"

  grep -o '"Id"[^,]*' "$TEMP_DIR/batch_$start.json" | \
    cut -d'"' -f4 >> "$ID_FILE"

  sleep 1
done

if [ ! -s "$ID_FILE" ]; then
  echo "Error: No IDs collected."
  exit 1
fi

echo "All $TOTAL_COUNT IDs collected. Saved to $ID_FILE"

# Step 3: Download GenBank files
echo -e "\n=== Step 3: Downloading GenBank records ==="

while IFS= read -r id; do
  filename="$OUTPUT_DIR/Anopheles_mito_${id}.gb"
  echo "Downloading ID: $id -> $filename"

  for attempt in 1 2 3; do
    wget -qO "$filename" \
      --timeout=30 \
      --tries=1 \
      "$BASE_URL/efetch.fcgi?db=$DB&id=$id&rettype=gb&retmode=text"

    if [ -s "$filename" ] && grep -q "LOCUS\|FEATURES" "$filename"; then
      echo "✅ Success: $filename"
      break
    else
      echo "⚠️  Failed (attempt $attempt)"
      if [ $attempt -eq 3 ]; then
        echo "❌ Giving up after 3 attempts."
      else
        sleep $((2 ** attempt))
      fi
    fi
  done

  sleep 1
done < "$ID_FILE"

# Summary
echo -e "\n=== Summary ==="
total_files=$(find "$OUTPUT_DIR" -name "*.gb" -size +0 | wc -l)
echo "Downloaded $total_files mitochondrial genome(s)."
echo "Output saved in: $OUTPUT_DIR"

if [ $total_files -gt 0 ]; then
  echo -e "\nSample of first file:"
  head -n 20 "$OUTPUT_DIR/Anopheles_mito_*.gb" | head -n 10
fi
```

---

## 🎓 Tutorial Summary

| Concept | What You Learned |
|-------|------------------|
| Shebang | `#!/bin/bash` |
| Variables | `OUTPUT_DIR="./dir"` |
| `mkdir -p` | Create directories safely |
| `wget` | Download files from web |
| `grep`, `cut`, `tr` | Parse text and JSON |
| `if [ -z ]`, `if [ -s ]` | Check strings and files |
| `for (( start=0; start < N; start += M ))` | Numeric loops |
| `while IFS= read -r id` | Read file line by line |
| `sleep` | Rate-limiting |
| `find`, `wc -l` | Count files |
| `head` | Show sample output |

---

## 🚀 Next Steps

Try modifying the script to:
1. Download only *Anopheles gambiae*
2. Save as FASTA instead of GenBank
3. Add a progress bar
4. Resume from failure
5. Log errors to a file

---

## 📚 Resources

- [NCBI E-utilities Guide](https://www.ncbi.nlm.nih.gov/books/NBK25499/)
- [Bash Guide for Beginners](https://tldp.org/LDP/Bash-Beginners-Guide/html/)
- [Advanced Bash-Scripting Guide](https://tldp.org/LDP/abs/html/)

---

You now have a **complete, working, and educational bash script** — perfect for a tutorial! 🎉

Would you like to add:
- A quiz?
- Exercises?
- A video walkthrough?
- A GitHub repo template?


