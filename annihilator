#!/bin/bash

#methods:


#---------------------------------------------------------------------------
#---------------------------------------------------------------------------
#---------------------------------------------------------------------------
corruptor ()
{
# Check if the user is root
if [ "$EUID" -ne 0 ]; then
    echo "Please run as root."
    exit 1
fi

# Function to modify a file with random information
modify_file() {
    local FILE="$1"
    local NUM_ENTRIES="$2"

    for i in $(seq 1 "$NUM_ENTRIES"); do
        ENTRY="Random entry #$i: $(shuf -i 1-1000 -n 1) - Random info: $(cat /dev/urandom | tr -dc 'A-Za-z0-9' | head -c 20)"
        echo "$ENTRY" >> "$FILE"  # Append instead of overwrite

        sleep 0.1
    done

    echo "File $FILE modified with $NUM_ENTRIES random entries."
}

# Function to corrupt a file
corrupt_file() {
    local file="$1"
    local times="$2"

    for ((i=0; i<times; i++)); do
        head -c 100 /dev/urandom > "$file"
        dd if=/dev/zero bs=1 count=50 of="$file" conv=notrunc 2>/dev/null
        printf '\xFF' | dd of="$file" bs=1 seek=10 conv=notrunc 2>/dev/null
        echo "Random data" >> "$file"
        truncate -s 0 "$file"
        > "$file"
        printf '\x00\x00\x00\x00' | dd of="$file" bs=1 count=4 conv=notrunc 2>/dev/null

        # Select a random file from the directory
        random_file=$(find "$DIR" -type f | shuf -n 1)
        dd if="$random_file" of="$file" bs=1 count=50 conv=notrunc 2>/dev/null
        printf '\xAA' | dd of="$file" bs=1 seek=5 conv=notrunc 2>/dev/null
    done
    echo "File $file corrupted $times times.";
}

# Function to randomize metadata
randomize_metadata() {
    local FILE="$1"

    RANDOM_DAYS_MODIFICATION=$((RANDOM % 731 + 730))
    RANDOM_DAYS_ACCESS=$((RANDOM % 731 + 730))
    RANDOM_DATE_MODIFICATION=$(date -d "-${RANDOM_DAYS_MODIFICATION} days" +"%Y-%m-%d %H:%M:%S")
    RANDOM_DATE_ACCESS=$(date -d "-${RANDOM_DAYS_ACCESS} days" +"%Y-%m-%d %H:%M:%S")

    # Modify the metadata
    touch -d "$RANDOM_DATE_MODIFICATION" "$FILE"
    touch -d "$RANDOM_DATE_ACCESS" "$FILE"

    # Notify the user
    #echo "Modified metadata for $FILE: Last modified set to $RANDOM_DATE_MODIFICATION, Last accessed set to $RANDOM_DATE_ACCESS."
}

randomize_directory_metadata() {
    local DIR="$1"

    RANDOM_DAYS_MODIFICATION=$((RANDOM % 731 + 730))
    RANDOM_DAYS_ACCESS=$((RANDOM % 731 + 730))
    RANDOM_DATE_MODIFICATION=$(date -d "-${RANDOM_DAYS_MODIFICATION} days" +"%Y-%m-%d %H:%M:%S")
    RANDOM_DATE_ACCESS=$(date -d "-${RANDOM_DAYS_ACCESS} days" +"%Y-%m-%d %H:%M:%S")

    # Modify the metadata
    touch -d "$RANDOM_DATE_MODIFICATION" "$DIR"
    touch -d "$RANDOM_DATE_ACCESS" "$DIR"

    # Notify the user
    #echo "Modified metadata for directory '$DIR': Last modified set to $RANDOM_DATE_MODIFICATION, Last accessed set to $RANDOM_DATE_ACCESS."
}

# Prompt the user for the directory path containing files to corrupt
echo "Corruptor: All files in a directory";
read -p "Enter the directory path containing files to corrupt: " DIR

# Check if the directory exists
if [ ! -d "$DIR" ]; then
    echo "The specified directory does not exist."
    exit 1
fi

# Prompt for the number of entries to generate
read -p "How many random entries would you like to generate? " NUM_ENTRIES

# Check if the number of entries is a valid number
if ! [[ "$NUM_ENTRIES" =~ ^[0-9]+$ ]]; then
    echo "Please enter a valid number."
    sleep 3
    exit 1
fi

# Prompt for the number of times to apply each corruption method
read -p "Enter the number of times to apply each corruption method: " times

# Validate that the input is a positive integer
if ! [[ "$times" =~ ^[0-9]+$ ]] || [ "$times" -le 0 ]; then
    echo "Please enter a valid positive integer."
    exit 1
fi

# Iterate over all files in the specified directory
echo
echo "Inserting randomly generated data entries into files";
find "$DIR" -type f | while read -r FILE; do
    modify_file "$FILE" "$NUM_ENTRIES"
done

echo
echo "Corrupting all files in the directory";
find "$DIR" -type f | while read -r FILE; do
    corrupt_file "$FILE" "$times"
    #randomize_metadata "$FILE"  # Randomize metadata after modification and corruption
done
#randomizing_metadata of all files to changes and access...
#find "$DIR" -type f | while read -r FILE; do
 #   echo "Randomly falsified metadata $NUM_ENTRIES times for: $FILE"
  #  randomize_metadata "$FILE"  # Randomize metadata after modification and corruption
#done


# Iterate over all files in the specified directory
echo
echo "Falsifying access and changing data of all files";
find "$DIR" -type f | while read -r FILE; do
    # Randomize metadata the same number of times as entries
    for ((i=0; i<NUM_ENTRIES; i++)); do
        randomize_metadata "$FILE"  # Randomize metadata after modification and corruption
    done
    #echo "Modified metadata for $FILE: $NUM_ENTRIES times | Last modified set to $RANDOM_DATE_MODIFICATION, Last accessed set to $RANDOM_DATE_ACCESS."
    # Notify the user after randomizing metadata
    echo "Modified metadata for $FILE: $NUM_ENTRIES times | Last modified set to $RANDOM_DATE_MODIFICATION, Last accessed set to $RANDOM_DATE_ACCESS."
done

echo;
# Randomize metadata again in directories...
echo "Falsifying access and changing data of all directories";
find "$DIR" -type d | while read -r DIRECTORY; do
    # Randomize metadata the same number of times as entries
    for ((i=0; i<NUM_ENTRIES; i++)); do
        randomize_directory_metadata "$DIRECTORY"  # Randomize metadata for directories
    done
    # Notify the user after randomizing metadata
    echo "Modified metadata for Directory $DIRECTORY: $NUM_ENTRIES times."
done


echo;
echo "File modification and corruption completed."
echo
}
#---------------------------------------------------------------------------
#---------------------------------------------------------------------------
#---------------------------------------------------------------------------

corruptor_files ()
{
# Check if the user is root
echo;
if [ "$EUID" -ne 0 ]; then
    echo "Please run as root."
    exit 1
fi

# Function to modify a file with random information
modify_file() {
    local FILE="$1"
    local NUM_ENTRIES="$2"

    for i in $(seq 1 "$NUM_ENTRIES"); do
        ENTRY="Random entry #$i: $(shuf -i 1-1000 -n 1) - Random info: $(cat /dev/urandom | tr -dc 'A-Za-z0-9' | head -c 20)"
        echo "$ENTRY" >> "$FILE"  # Append instead of overwrite

        sleep 0.1
    done

    echo "File $FILE modified with $NUM_ENTRIES random entries."
}

# Function to corrupt a file
corrupt_file() {
    local file="$1"
    local times="$2"

    for ((i=0; i<times; i++)); do
        head -c 100 /dev/urandom > "$file"
        dd if=/dev/zero bs=1 count=50 of="$file" conv=notrunc 2>/dev/null
        printf '\xFF' | dd of="$file" bs=1 seek=10 conv=notrunc 2>/dev/null
        echo "Random data" >> "$file"
        truncate -s 0 "$file"
        > "$file"
        printf '\x00\x00\x00\x00' | dd of="$file" bs=1 count=4 conv=notrunc 2>/dev/null
    done
    echo "File $file corrupted $times times.";
}

# Function to randomize metadata
randomize_metadata() {
    local FILE="$1"

    RANDOM_DAYS_MODIFICATION=$((RANDOM % 731 + 730))
    RANDOM_DAYS_ACCESS=$((RANDOM % 731 + 730))
    RANDOM_DATE_MODIFICATION=$(date -d "-${RANDOM_DAYS_MODIFICATION} days" +"%Y-%m-%d %H:%M:%S")
    RANDOM_DATE_ACCESS=$(date -d "-${RANDOM_DAYS_ACCESS} days" +"%Y-%m-%d %H:%M:%S")

    # Modify the metadata
    touch -d "$RANDOM_DATE_MODIFICATION" "$FILE"
    touch -d "$RANDOM_DATE_ACCESS" "$FILE"

    # Notify the user
    #echo "Modified metadata for $FILE: Last modified set to $RANDOM_DATE_MODIFICATION, Last accessed set to $RANDOM_DATE_ACCESS."
}

randomize_directory_metadata() {
    local DIR="$1"

    RANDOM_DAYS_MODIFICATION=$((RANDOM % 731 + 730))
    RANDOM_DAYS_ACCESS=$((RANDOM % 731 + 730))
    RANDOM_DATE_MODIFICATION=$(date -d "-${RANDOM_DAYS_MODIFICATION} days" +"%Y-%m-%d %H:%M:%S")
    RANDOM_DATE_ACCESS=$(date -d "-${RANDOM_DAYS_ACCESS} days" +"%Y-%m-%d %H:%M:%S")

    # Modify the metadata
    touch -d "$RANDOM_DATE_MODIFICATION" "$DIR"
    touch -d "$RANDOM_DATE_ACCESS" "$DIR"

    # Notify the user
    #echo "Modified metadata for directory '$DIR': Last modified set to $RANDOM_DATE_MODIFICATION, Last accessed set to $RANDOM_DATE_ACCESS."
}


# Prompt the user for the file path to modify and corrupt

echo
echo "Corruptor File";
read -p "Enter the path with the file name to modify and corrupt: " FILE
echo "You entered: $FILE"

# Prompt for the number of entries to generate
read -p "How many random entries would you like to generate? " NUM_ENTRIES

# Check if the number of entries is a valid number
if ! [[ "$NUM_ENTRIES" =~ ^[0-9]+$ ]]; then
    echo "Please enter a valid number."
    sleep 3
    exit 1
fi

# Prompt for the number of times to apply each corruption method
read -p "Enter the number of times to apply each corruption method: " times

# Validate that the input is a positive integer
if ! [[ "$times" =~ ^[0-9]+$ ]] || [ "$times" -le 0 ]; then
    echo "Please enter a valid positive integer."
    exit 1
fi

# Modify the specified file
modify_file "$FILE" "$NUM_ENTRIES"
corrupt_file "$FILE" "$times"

# Randomize metadata the same number of times as entries
for ((i=0; i<NUM_ENTRIES; i++)); do
    randomize_metadata "$FILE"  # Randomize metadata after modification and corruption
done
echo "Modified metadata for $FILE: $NUM_ENTRIES times | Last modified set to $RANDOM_DATE_MODIFICATION, Last accessed set to $RANDOM_DATE_ACCESS."

#echo;
# Randomize metadata again in directories...
#find "$DIR" -type d | while read -r DIRECTORY; do
    # Randomize metadata the same number of times as entries
#    for ((i=0; i<NUM_ENTRIES; i++)); do
#        randomize_directory_metadata "$DIRECTORY"  # Randomize metadata for directories
#    done
    # Notify the user after randomizing metadata
#    echo "Modified metadata for Directory $DIRECTORY: $NUM_ENTRIES times."
#done

echo "File modification and corruption completed."
echo
}
#---------------------------------------------------------------------------
#---------------------------------------------------------------------------
#---------------------------------------------------------------------------


logstorm ()
{
echo "Log Storm (basic forensic sabotage";
# Check if the user is root
if [ "$EUID" -ne 0 ]; then
    echo "Please run as root."
    exit 1
fi

# Function to modify a log file with random information and randomize metadata
modify_log() {
    local FILE="$1"
    local NUM_ENTRIES="$2"

    # Generate random entries and overwrite the log file
    for i in $(seq 1 "$NUM_ENTRIES"); do
        # Generate a random entry without a timestamp
        ENTRY="Random log #$i: $(shuf -i 1-1000 -n 1) - Random info: $(cat /dev/urandom | tr -dc 'A-Za-z0-9' | head -c 20)"
        
        # Overwrite the log file with the new entry
        echo "$ENTRY" > "$FILE"
        
        # Generate a random number of days in the past (up to 364 days)
        RANDOM_DAYS_MODIFICATION=$((RANDOM % 365))  # Random number of days for modification
        RANDOM_DAYS_ACCESS=$((RANDOM % 365))         # Random number of days for access
        
        # Generate random dates in the past
        RANDOM_DATE_MODIFICATION=$(date -d "-${RANDOM_DAYS_MODIFICATION} days" +"%Y-%m-%d %H:%M:%S")
        RANDOM_DATE_ACCESS=$(date -d "-${RANDOM_DAYS_ACCESS} days" +"%Y-%m-%d %H:%M:%S")

        # Set the random dates as the modification and access times
        touch -d "$RANDOM_DATE_MODIFICATION" "$FILE"  # Modify timestamp
        touch -d "$RANDOM_DATE_ACCESS" "$FILE"        # Access timestamp

        # Optional: Sleep for a short time to simulate time between entries
        sleep 0.1
    done

    echo "File $FILE modified with $NUM_ENTRIES random entries."
}

# Prompt the user for the log directory
echo "Choose the log directory option:"
echo "1) Use default /var/log"
echo "2) Specify a different directory"
read -p "Enter your choice (1 or 2): " DIR_OPTION

if [ "$DIR_OPTION" -eq 1 ]; then
    LOG_DIR="/var/log"
elif [ "$DIR_OPTION" -eq 2 ]; then
    read -p "Enter the log directory: " USER_LOG_DIR
    LOG_DIR="$USER_LOG_DIR"
else
    echo "Invalid option. Exiting."
    sleep 3
    exit 1
fi

# Prompt for the number of entries to generate
read -p "How many random entries would you like to generate? " NUM_ENTRIES

# Check if the number of entries is a valid number
if ! [[ "$NUM_ENTRIES" =~ ^[0-9]+$ ]]; then
    echo "Please enter a valid number."
    sleep 3
    exit 1
fi

# Iterate over all log files in the specified directory
find "$LOG_DIR" -type f | while read -r FILE; do
    modify_log "$FILE" "$NUM_ENTRIES"
done
echo
}


#---------------------------------------------------------------------------
#---------------------------------------------------------------------------
#---------------------------------------------------------------------------
renamer_oblivion ()
{
echo "Renamer Oblivion"
# Check if the user is root
echo
echo "Renamer Oblivion"
if [ "$EUID" -ne 0 ]; then
    echo "Please run as root"
    exit 1
fi

# Function to generate a random alphanumeric string
generate_random_name() {
    local length=$1
    tr -dc 'A-Za-z0-9' < /dev/urandom | head -c "$length"
}

# List of common file extensions
extensions=("txt" "jpg" "png" "gif" "pdf" "doc" "docx" "xls" "xlsx" "ppt" "pptx" "zip" "tar" "gz" "mp3" "mp4" "avi" "html" "css" "js" "json" "xml")

# Prompt for the directory
read -p "Enter the directory path: " dir_path

# Check if the directory exists
if [ ! -d "$dir_path" ]; then
    echo "Directory does not exist."
    exit 1
fi

# Prompt for the number of modifications
read -p "How many modifications do you want to make for each file and directory? " num_modifications

# Initialize an associative array to track modifications
declare -A modified_files

# Find and rename files
find "$dir_path" -type f | while read -r file; do
    original_file="$file"
    final_file="$file"  # Start with the original file

    for ((i=0; i<num_modifications; i++)); do
        # Generate a random name and select a random extension
        random_name=$(generate_random_name $((RANDOM % 31 + 6)))  # Length between 6 and 36
        random_extension=${extensions[RANDOM % ${#extensions[@]}]}
        
        # Get the directory of the file
        file_dir=$(dirname "$final_file")
        
        # Construct the new file name
        new_file="$file_dir/$random_name.$random_extension"
        
        # Rename the file, suppressing error messages
        mv "$final_file" "$new_file" 2>/dev/null
        
        # Update the final file variable to the new file for further modifications
        final_file="$new_file"
    done
    
    # Print the original and final new file names
    echo "Original file: $original_file -> Final file: $final_file - Modified $num_modifications times"
done

# Find and rename directories, excluding the top-level directory
find "$dir_path" -mindepth 1 -type d | while read -r dir; do
    original_dir="$dir"
    final_dir="$dir"  # Start with the original directory

    for ((i=0; i<num_modifications; i++)); do
        # Generate a random name (no extension for directories)
        random_name=$(generate_random_name $((RANDOM % 31 + 6)))  # Length between 6 and 36
        
        # Get the parent directory of the current directory
        parent_dir=$(dirname "$final_dir")
        
        # Construct the new directory name
        new_dir="$parent_dir/$random_name"
        
        # Rename the directory, suppressing error messages
        mv "$final_dir" "$new_dir" 2>/dev/null
        
        # Update the final directory variable to the new directory for further modifications
        final_dir="$new_dir"
    done
    
    # Print the original and final new directory names
    echo "Original directory: $original_dir -> Final directory: $final_dir - Modified $num_modifications times"
done
echo
}
#---------------------------------------------------------------------------
#---------------------------------------------------------------------------
#---------------------------------------------------------------------------

renamer_oblivion_file ()
{
# Check if the user is root
echo
echo "Renamer Oblivion for one file"
if [ "$EUID" -ne 0 ]; then
    echo "Please run as root"
    exit 1
fi

# Function to generate a random alphanumeric string
generate_random_name() {
    local length=$1
    tr -dc 'A-Za-z0-9' < /dev/urandom | head -c "$length"
}

# List of common file extensions
extensions=("txt" "jpg" "png" "gif" "pdf" "doc" "docx" "xls" "xlsx" "ppt" "pptx" "zip" "tar" "gz" "mp3" "mp4" "avi" "html" "css" "js" "json" "xml")

# Prompt for the file
    read -p "Enter the path with the file name: " file_path
    echo "You entered: $file_path"


# Prompt for the number of modifications
read -p "How many modifications do you want to make for the file? " num_modifications

# Store the original file name
original_file="$file_path"
final_file="$file_path"  # Start with the original file

# Perform the modifications
for ((i=0; i<num_modifications; i++)); do
    # Generate a random name and select a random extension
    random_name=$(generate_random_name $((RANDOM % 31 + 6)))  # Length between 6 and 36
    random_extension=${extensions[RANDOM % ${#extensions[@]}]}
    
    # Get the directory of the file
    file_dir=$(dirname "$final_file")
    
    # Construct the new file name
    new_file="$file_dir/$random_name.$random_extension"
    
    # Rename the file
    mv "$final_file" "$new_file"
    
    # Update the final file variable to the new file for further modifications
    final_file="$new_file"
done

# Print the original and final new file names
echo "Original file: $original_file -> Final file: $final_file - Modified $num_modifications times"

echo
}
#---------------------------------------------------------------------------
#---------------------------------------------------------------------------
#---------------------------------------------------------------------------

renamer_oblivion_dir ()
{

# Check if the user is root
echo
echo "Renamer Oblivion for one directory"
if [ "$EUID" -ne 0 ]; then
    echo "Please run as root"
    exit 1
fi

# Function to generate a random alphanumeric string
generate_random_name() {
    local length=$1
    tr -dc 'A-Za-z0-9' < /dev/urandom | head -c "$length"
}

# Prompt for the directory
read -p "Enter the directory path: " dir_path

# Check if the directory exists
if [ ! -d "$dir_path" ]; then
    echo "Directory does not exist."
    exit 1
fi

# Prompt for the number of modifications
read -p "How many modifications do you want to make for the directory? " num_modifications

# Store the original directory name
original_dir="$dir_path"
final_dir="$dir_path"  # Start with the original directory

# Perform the modifications
for ((i=0; i<num_modifications; i++)); do
    # Generate a random name (no extension for directories)
    random_name=$(generate_random_name $((RANDOM % 31 + 6)))  # Length between 6 and 36
    
    # Get the parent directory of the current directory
    parent_dir=$(dirname "$final_dir")
    
    # Construct the new directory name
    new_dir="$parent_dir/$random_name"
    
    # Rename the directory
    mv "$final_dir" "$new_dir" 2>/dev/null
    
    # Update the final directory variable to the new directory for further modifications
    final_dir="$new_dir"
done

# Print the original and final new directory names
echo "Original directory: $original_dir -> Final directory: $final_dir - Modified $num_modifications times"
echo
}
#---------------------------------------------------------------------------
#---------------------------------------------------------------------------
#---------------------------------------------------------------------------


mega_ultra_corruptor ()
{
# Check if the user is root
if [ "$EUID" -ne 0 ]; then
    echo "Please run as root."
    exit 1
fi


#methods and functions...
# Function to modify a file with random information
modify_file() {
    local FILE="$1"
    local NUM_ENTRIES="$2"

    for i in $(seq 1 "$NUM_ENTRIES"); do
        ENTRY="Random entry #$i: $(shuf -i 1-1000 -n 1) - Random info: $(cat /dev/urandom | tr -dc 'A-Za-z0-9' | head -c 20)"
        echo "$ENTRY" >> "$FILE"  # Append instead of overwrite

        sleep 0.1
    done

    echo "File $FILE modified with $NUM_ENTRIES random entries."
}

# Function to corrupt a file
corrupt_file() {
    local file="$1"
    local times="$2"

    for ((i=0; i<times; i++)); do
        head -c 100 /dev/urandom > "$file"
        dd if=/dev/zero bs=1 count=50 of="$file" conv=notrunc 2>/dev/null
        printf '\xFF' | dd of="$file" bs=1 seek=10 conv=notrunc 2>/dev/null
        echo "Random data" >> "$file"
        truncate -s 0 "$file"
        > "$file"
        printf '\x00\x00\x00\x00' | dd of="$file" bs=1 count=4 conv=notrunc 2>/dev/null

        # Select a random file from the directory
        random_file=$(find "$DIR" -type f | shuf -n 1)
        dd if="$random_file" of="$file" bs=1 count=50 conv=notrunc 2>/dev/null
        printf '\xAA' | dd of="$file" bs=1 seek=5 conv=notrunc 2>/dev/null
    done
    echo "File $file corrupted $times times.";
}

# Function to randomize metadata
randomize_metadata() {
    local FILE="$1"

    RANDOM_DAYS_MODIFICATION=$((RANDOM % 731 + 730))
    RANDOM_DAYS_ACCESS=$((RANDOM % 731 + 730))
    RANDOM_DATE_MODIFICATION=$(date -d "-${RANDOM_DAYS_MODIFICATION} days" +"%Y-%m-%d %H:%M:%S")
    RANDOM_DATE_ACCESS=$(date -d "-${RANDOM_DAYS_ACCESS} days" +"%Y-%m-%d %H:%M:%S")

    # Modify the metadata
    touch -d "$RANDOM_DATE_MODIFICATION" "$FILE"
    touch -d "$RANDOM_DATE_ACCESS" "$FILE"

    # Notify the user
    #echo "Modified metadata for $FILE: Last modified set to $RANDOM_DATE_MODIFICATION, Last accessed set to $RANDOM_DATE_ACCESS."
}

randomize_directory_metadata() {
    local DIR="$1"

    RANDOM_DAYS_MODIFICATION=$((RANDOM % 731 + 730))
    RANDOM_DAYS_ACCESS=$((RANDOM % 731 + 730))
    RANDOM_DATE_MODIFICATION=$(date -d "-${RANDOM_DAYS_MODIFICATION} days" +"%Y-%m-%d %H:%M:%S")
    RANDOM_DATE_ACCESS=$(date -d "-${RANDOM_DAYS_ACCESS} days" +"%Y-%m-%d %H:%M:%S")

    # Modify the metadata
    touch -d "$RANDOM_DATE_MODIFICATION" "$DIR"
    touch -d "$RANDOM_DATE_ACCESS" "$DIR"

    # Notify the user
    #echo "Modified metadata for directory '$DIR': Last modified set to $RANDOM_DATE_MODIFICATION, Last accessed set to $RANDOM_DATE_ACCESS."
}

#renamer_oblivion data and methods
# Function to generate a random alphanumeric string for renamer_oblivion
generate_random_name() {
    local length=$1
    tr -dc 'A-Za-z0-9' < /dev/urandom | head -c "$length"
}

# List of common file extensions for renamer_oblivion
extensions=("txt" "jpg" "png" "gif" "pdf" "doc" "docx" "xls" "xlsx" "ppt" "pptx" "zip" "tar" "gz" "mp3" "mp4" "avi" "html" "css" "js" "json" "xml")

renamer ()
{
# Initialize an associative array to track modifications
declare -A modified_files

# Find and rename files
find "$DIR" -type f | while read -r file; do
    original_file="$file"
    final_file="$file"  # Start with the original file

    for ((i=0; i<NUM_ENTRIES; i++)); do
        # Generate a random name and select a random extension
        random_name=$(generate_random_name $((RANDOM % 31 + 6)))  # Length between 6 and 36
        random_extension=${extensions[RANDOM % ${#extensions[@]}]}
        
        # Get the directory of the file
        file_dir=$(dirname "$final_file")
        
        # Construct the new file name
        new_file="$file_dir/$random_name.$random_extension"
        
        # Rename the file, suppressing error messages
        mv "$final_file" "$new_file" 2>/dev/null
        
        # Update the final file variable to the new file for further modifications
        final_file="$new_file"
    done
    
    # Print the original and final new file names
    echo "Original file: $original_file -> Final file: $final_file - Modified $NUM_ENTRIES times"
done

# Find and rename directories, excluding the top-level directory
find "$DIR" -mindepth 1 -type d | while read -r dir; do
    original_dir="$dir"
    final_dir="$dir"  # Start with the original directory

    for ((i=0; i<NUM_ENTRIES; i++)); do
        # Generate a random name (no extension for directories)
        random_name=$(generate_random_name $((RANDOM % 31 + 6)))  # Length between 6 and 36
        
        # Get the parent directory of the current directory
        parent_dir=$(dirname "$final_dir")
        
        # Construct the new directory name
        new_dir="$parent_dir/$random_name"
        
        # Rename the directory, suppressing error messages
        mv "$final_dir" "$new_dir" 2>/dev/null
        
        # Update the final directory variable to the new directory for further modifications
        final_dir="$new_dir"
    done
    
    # Print the original and final new directory names
    echo "Original directory: $original_dir -> Final directory: $final_dir - Modified $NUM_ENTRIES times"
done
}

####################################################################
#STARTING
####################################################################
# Prompt the user for the directory path containing files to corrupt
echo "Mega Ultra Corruptor";
read -p "Enter the directory path containing files to corrupt: " DIR

# Check if the directory exists
if [ ! -d "$DIR" ]; then
    echo "The specified directory does not exist."
    exit 1
fi

# Prompt for the number of entries to generate
read -p "How many random entries would you like to generate? " NUM_ENTRIES

# Check if the number of entries is a valid number
if ! [[ "$NUM_ENTRIES" =~ ^[0-9]+$ ]]; then
    echo "Please enter a valid number."
    sleep 3
    exit 1
fi

# Prompt for the number of times to apply each corruption method
read -p "Enter the number of times to apply each corruption method: " times

# Validate that the input is a positive integer
if ! [[ "$times" =~ ^[0-9]+$ ]] || [ "$times" -le 0 ]; then
    echo "Please enter a valid positive integer."
    exit 1
fi

echo;
echo "Inserting randomly generated data entries into files";
# Iterate over all files in the specified directory
find "$DIR" -type f | while read -r FILE; do
    modify_file "$FILE" "$NUM_ENTRIES"
done

echo
echo "Corrupting all files in the directory";
find "$DIR" -type f | while read -r FILE; do
    corrupt_file "$FILE" "$times"
    #randomize_metadata "$FILE"  # Randomize metadata after modification and corruption
done

#randomizing_metadata of all files to changes and access...
#find "$DIR" -type f | while read -r FILE; do
 #   echo "Randomly falsified metadata $NUM_ENTRIES times for: $FILE"
  #  randomize_metadata "$FILE"  # Randomize metadata after modification and corruption
#done

echo;
echo "Falsifying access and changing data of all files";
# Iterate over all files in the specified directory
find "$DIR" -type f | while read -r FILE; do
    # Randomize metadata the same number of times as entries
    for ((i=0; i<NUM_ENTRIES; i++)); do
        randomize_metadata "$FILE"  # Randomize metadata after modification and corruption
    done
    #echo "Modified metadata for $FILE: $NUM_ENTRIES times | Last modified set to $RANDOM_DATE_MODIFICATION, Last accessed set to $RANDOM_DATE_ACCESS."
    # Notify the user after randomizing metadata
    echo "Modified metadata for $FILE: $NUM_ENTRIES times | Last modified set to $RANDOM_DATE_MODIFICATION, Last accessed set to $RANDOM_DATE_ACCESS."
done

echo;
echo "Falsifying access and changing data of all directories";
# Randomize metadata again in directories...
find "$DIR" -type d | while read -r DIRECTORY; do
    # Randomize metadata the same number of times as entries
    for ((i=0; i<NUM_ENTRIES; i++)); do
        randomize_directory_metadata "$DIRECTORY"  # Randomize metadata for directories
    done
    # Notify the user after randomizing metadata
    echo "Modified metadata for Directory $DIRECTORY: $NUM_ENTRIES times."
done

echo;
#starting renamer_oblivion
echo "Renamer Oblivion: Renaming files and extensions randomly"
renamer

echo;
echo "Again! Falsifying access and changing data of all files";
# randomize_metadata again...
find "$DIR" -type f | while read -r FILE; do
    # Randomize metadata the same number of times as entries
    for ((i=0; i<NUM_ENTRIES; i++)); do
        randomize_metadata "$FILE"  # Randomize metadata after modification and corruption
    done
    #echo "Modified metadata for $FILE: $NUM_ENTRIES times | Last modified set to $RANDOM_DATE_MODIFICATION, Last accessed set to $RANDOM_DATE_ACCESS."
    # Notify the user after randomizing metadata
    echo "Modified metadata for $FILE: $NUM_ENTRIES times | Last modified set to $RANDOM_DATE_MODIFICATION, Last accessed set to $RANDOM_DATE_ACCESS."
done

echo;
echo "Again! Falsifying access and changing data of all directories";
# Randomize metadata again in directories...
find "$DIR" -type d | while read -r DIRECTORY; do
    # Randomize metadata the same number of times as entries
    for ((i=0; i<NUM_ENTRIES; i++)); do
        randomize_directory_metadata "$DIRECTORY"  # Randomize metadata for directories
    done
    # Notify the user after randomizing metadata
    echo "Modified metadata for Directory $DIRECTORY: $NUM_ENTRIES times."
done

echo;
echo "File modification and corruption completed."
echo
}

#---------------------------------------------------------------------------
#---------------------------------------------------------------------------
#---------------------------------------------------------------------------
metadata_changer_file ()
{
# Function to randomize metadata of a file
randomize_metadata() {
    local FILE="$1"
    local COUNT="$2"

    for ((i=0; i<COUNT; i++)); do
        RANDOM_DAYS_MODIFICATION=$((RANDOM % 731 + 730))
        RANDOM_DAYS_ACCESS=$((RANDOM % 731 + 730))
        RANDOM_DATE_MODIFICATION=$(date -d "-${RANDOM_DAYS_MODIFICATION} days" +"%Y-%m-%d %H:%M:%S")
        RANDOM_DATE_ACCESS=$(date -d "-${RANDOM_DAYS_ACCESS} days" +"%Y-%m-%d %H:%M:%S")

        # Modify the metadata
        touch -d "$RANDOM_DATE_MODIFICATION" "$FILE"
        touch -d "$RANDOM_DATE_ACCESS" "$FILE"
    done

    # Notify the user
    echo "Modified metadata for $FILE $COUNT times."
}

# Prompt the user for the file path
echo "Metadata Changer File";
read -p "Enter the path with the file name: " FILE
# You can add your file modification logic here
echo "You entered: $FILE"

# Prompt for the number of modifications
read -p "How many times do you want to modify the metadata for this file? " COUNT

# Confirm the operation
read -p "Are you sure you want to modify the metadata of '$FILE' $COUNT times? (y/n): " CONFIRM
if [[ ! "$CONFIRM" =~ ^[yY]$ ]]; then
    echo "Operation canceled."
    exit 0
fi

# Modify the metadata of the specified file
randomize_metadata "$FILE" "$COUNT"

echo "Metadata modification completed."
echo
}

#---------------------------------------------------------------------------
#---------------------------------------------------------------------------
#---------------------------------------------------------------------------
metadata_changer_dir ()
{
# Function to randomize metadata of a file
randomize_metadata() {
    local FILE="$1"
    local COUNT="$2"

    for ((i=0; i<COUNT; i++)); do
        RANDOM_DAYS_MODIFICATION=$((RANDOM % 731 + 730))
        RANDOM_DAYS_ACCESS=$((RANDOM % 731 + 730))
        RANDOM_DATE_MODIFICATION=$(date -d "-${RANDOM_DAYS_MODIFICATION} days" +"%Y-%m-%d %H:%M:%S")
        RANDOM_DATE_ACCESS=$(date -d "-${RANDOM_DAYS_ACCESS} days" +"%Y-%m-%d %H:%M:%S")

        # Modify the metadata
        touch -d "$RANDOM_DATE_MODIFICATION" "$FILE"
        touch -d "$RANDOM_DATE_ACCESS" "$FILE"
    done

    # Notify the user
    echo "Modified metadata for $FILE $COUNT times."
}

# Function to randomize metadata of directories
randomize_directory_metadata() {
    local DIR="$1"
    local COUNT="$2"

    for ((i=0; i<COUNT; i++)); do
        RANDOM_DAYS_MODIFICATION=$((RANDOM % 731 + 730))
        RANDOM_DAYS_ACCESS=$((RANDOM % 731 + 730))
        RANDOM_DATE_MODIFICATION=$(date -d "-${RANDOM_DAYS_MODIFICATION} days" +"%Y-%m-%d %H:%M:%S")
        RANDOM_DATE_ACCESS=$(date -d "-${RANDOM_DAYS_ACCESS} days" +"%Y-%m-%d %H:%M:%S")

        # Modify the metadata
        touch -d "$RANDOM_DATE_MODIFICATION" "$DIR"
        touch -d "$RANDOM_DATE_ACCESS" "$DIR"
    done

    # Notify the user
    echo "Modified metadata for directory '$DIR' $COUNT times."
}

# Prompt the user for the directory path
echo "Metadata Changer Dir"
read -p "Enter the path of the directory containing the files: " DIR

# Check if the directory exists
if [ ! -d "$DIR" ]; then
    echo "The specified directory does not exist."
    exit 1
fi

# Prompt for the number of modifications
read -p "How many times do you want to modify the metadata for each file and directory? " COUNT

# Confirm the operation
read -p "Are you sure you want to modify the metadata of all files and directories in '$DIR' $COUNT times? (y/n): " CONFIRM
if [[ ! "$CONFIRM" =~ ^[yY]$ ]]; then
    echo "Operation canceled."
    exit 0
fi

# Iterate over all files in the specified directory
find "$DIR" -type f | while read -r FILE; do
    randomize_metadata "$FILE" "$COUNT"
done

# Iterate over all directories in the specified directory
find "$DIR" -type d | while read -r DIRECTORY; do
    randomize_directory_metadata "$DIRECTORY" "$COUNT"
done

echo "Metadata modification completed."
echo
}

#---------------------------------------------------------------------------
#---------------------------------------------------------------------------
#---------------------------------------------------------------------------
shred_dir()
{
# Function to display usage information
usage() {
    echo "Shred options in use:"
    echo "  -v : Verbose mode, shows progress."
    echo "  -z : Add a final overwrite with zeros to hide shredding."
    echo "  -u : Remove the file after overwriting."
    echo "Example: $0"
    exit 1
}

# Check if shred is installed
if ! command -v shred &> /dev/null; then
    echo "Error: shred is not installed. Please install it and try again."
    echo "You can install it using: sudo apt install shred -y"
    sleep 4
    exit 1
fi

# Prompt user for the target (file or directory)
echo
echo "Shred Secure Erasing Files";
read -p "Enter the absolute path to the file or directory you want to shred: " target
echo "You entered: $target"  # Corrected variable name


# Prompt user for the number of times to run shred
echo
read -p "Enter the number of times to run shred (positive integer): " times

# Validate the number of times
if ! [[ "$times" =~ ^[0-9]+$ ]]; then
    echo "Error: The number of times must be a positive integer."
    exit 1
fi

# Check if the target is a file or directory
if [ -f "$target" ]; then
    echo "Shredding file: "$target""
    shred -vzu -n "$times" "$target"
elif [ -d "$target" ]; then
    echo "Shredding all files in directory: $target"
    find "$target" -type f -exec shred -vzu -n "$times" {} \;
else
    echo "Error: $target is neither a file nor a directory."
    exit 1
fi

echo "Shredding completed."
echo
}



#---------------------------------------------------------------------------
#---------------------------------------------------------------------------
#---------------------------------------------------------------------------
zero_urandom_fill()
{
#!/bin/bash

# Check if the user is root
check_root() {
    if [ "$EUID" -ne 0 ]; then
        echo "This script must be run as root. Please run it with sudo."
        exit 1
    fi
}

# Function to list available partitions
list_partitions() {
    echo "Available partitions:"
    lsblk -f
    echo -e "\nYou can also check partitions using gnome-disks manually."
}

# Function to explain secure erase methods
explain_methods() {
    echo -e "\nSecure Erase Methods:"
    echo "1. GOST R 50739-95: This is a Russian standard for data sanitization that involves multiple overwriting passes to ensure data is irretrievable. Use 1 pass!"
    echo "2. Gutmann Method: This method involves 35 passes of overwriting data with various patterns to ensure complete data destruction. Use 35 passes!"
}

# Function to get user input for partition
get_partition() {
    read -p "Please enter the partition you wish to secure erase (e.g., /dev/sdX): " partition
    echo $partition
}

# Function to check if the partition is mounted and unmount it if necessary
check_and_unmount() {
    local partition=$1  # Declare a local variable
    echo "Attempting to unmount the partition $partition..."
    read -p "Please make sure there are no applications using this partition. Are you sure you want to unmount $partition? (yes/no): " confirm
    if [[ "$confirm" == "yes" ]]; then
        sudo umount -f "$partition"  # Force unmount
        if [ $? -ne 0 ]; then
            echo "Failed to unmount $partition. Please ensure it is not in use."
            exit 1
        fi
        echo "$partition has been unmounted."
    else
        echo "Operation canceled. The partition $partition was not unmounted."
        sleep 4
        exit 1
    fi
}



# Function to get user input for number of passes
get_pass_count() {
    read -p "How many passes would you like to perform? (Default is 1 for zeroing and random): " count
    if [[ -z "$count" ]]; then
        count=1
    fi
    echo $count
}

# Function to get user input for block size
get_block_size() {
    read -p "Enter block size (bs) in bytes (default is 4096): " block_size
    if [[ -z "$block_size" ]]; then
        block_size=4096
    fi
    echo $block_size
}

# Function to confirm and execute the commands
confirm_and_execute() {
    local partition=$1
    local passes=$2
    local block_size=$3

    echo -e "\nYou have chosen to perform $passes passes on $partition with block size $block_size."
    echo "Commands to be executed:"

    commands=()
    for ((i=0; i<passes; i++)); do
        commands+=("sudo dd if=/dev/zero of=$partition bs=$block_size status=progress")
        commands+=("sudo dd if=/dev/urandom of=$partition bs=$block_size status=progress")
       
    done

    for command in "${commands[@]}"; do
        echo "$command"
    done

    read -p "Are you sure you want to proceed? (yes/no): " proceed
    if [[ "$proceed" == "yes" ]]; then
        for command in "${commands[@]}"; do
            eval $command
        done
        echo "Secure erase completed."
    else
        echo "Operation canceled."
    fi
}

# Main script execution
echo
check_root
echo "GOST R 50739-95 & Gutmann Secure Erase for partition or HDD (/dev/zero & /dev/urandom | only on HDD)";
echo "This program will perform a secure erase on the selected partition."
echo "Use it only on HDDs!"
explain_methods
echo
list_partitions
echo

partition=$(get_partition)
check_and_unmount "$partition"  # Check and unmount the partition
passes=$(get_pass_count)
block_size=$(get_block_size)

confirm_and_execute "$partition" "$passes" "$block_size"
echo
}

#---------------------------------------------------------------------------
#---------------------------------------------------------------------------
#---------------------------------------------------------------------------

linux_logs_corruptor_master()
{

linux_logs_corruptor_online() {
  # Check if the user is root
  if [ "$EUID" -ne 0 ]; then
    echo "Please run as root."
    exit 1
  fi

  # Function to modify a file with random information
  modify_file() {
    local FILE="$1"
    local NUM_ENTRIES="$2"
    for i in $(seq 1 "$NUM_ENTRIES"); do
      ENTRY="Random entry #$i: $(shuf -i 1-1000 -n 1) - Random info: $(cat /dev/urandom | tr -dc 'A-Za-z0-9' | head -c 20)"
      echo "$ENTRY" >> "$FILE"
      sleep 0.1
    done
    echo "File $FILE modified with $NUM_ENTRIES random entries."
  }

  # Function to corrupt a file
  corrupt_file() {
    local file="$1"
    local times="$2"
    for ((i=0; i<times; i++)); do
      head -c 100 /dev/urandom > "$file"
      dd if=/dev/zero bs=1 count=50 of="$file" conv=notrunc 2>/dev/null
      printf '\xFF' | dd of="$file" bs=1 seek=10 conv=notrunc 2>/dev/null
      echo "Random data" >> "$file"
      truncate -s 0 "$file" > "$file"
      printf '\x00\x00\x00\x00' | dd of="$file" bs=1 count=4 conv=notrunc 2>/dev/null
      random_file=$(find "/var/log" -type f | shuf -n 1)
      dd if="$random_file" of="$file" bs=1 count=50 conv=notrunc 2>/dev/null
      printf '\xAA' | dd of="$file" bs=1 seek=5 conv=notrunc 2>/dev/null
    done
    echo "File $file corrupted $times times."
  }

  # Function to randomize metadata
  randomize_metadata() {
    local FILE="$1"
    RANDOM_DAYS_MODIFICATION=$((RANDOM % 731 + 730))
    RANDOM_DAYS_ACCESS=$((RANDOM % 731 + 730))
    RANDOM_DATE_MODIFICATION=$(date -d "-${RANDOM_DAYS_MODIFICATION} days" +"%Y-%m-%d %H:%M:%S")
    RANDOM_DATE_ACCESS=$(date -d "-${RANDOM_DAYS_ACCESS} days" +"%Y-%m-%d %H:%M:%S")
    touch -d "$RANDOM_DATE_MODIFICATION" "$FILE"
    touch -d "$RANDOM_DATE_ACCESS" "$FILE"
  }

  # Prompt the user for the username
  read -p "Enter your linux user: " USERNAME

  # Prompt for the number of entries to generate
  read -p "How many random entries would you like to generate? " NUM_ENTRIES

  # Prompt for the number of times to apply each corruption method
  read -p "Enter the number of times to apply each corruption method: " TIMES

  # Validate inputs
  if ! [[ "$NUM_ENTRIES" =~ ^[0-9]+$ ]]; then
    echo "Please enter a valid number."
    sleep 3
    exit 1
  fi
  if ! [[ "$TIMES" =~ ^[0-9]+$ ]] || [ "$TIMES" -le 0 ]; then
    echo "Please enter a valid positive integer."
    sleep 3
    exit 1
  fi

  # Apply the algorithm to /var/log
  echo
  echo "/var/log corruptions attack starting"
  echo "Inserting randomly generated data entries into files"
  find "/var/log" -type f | while read -r FILE; do
    modify_file "$FILE" "$NUM_ENTRIES"
  done
  echo
  echo "Corrupting all files in the directory"
  find "/var/log" -type f | while read -r FILE; do
    corrupt_file "$FILE" "$TIMES"
  done
  echo
  echo "Falsifying access and changing data of all files"
  find "/var/log" -type f | while read -r FILE; do
    for ((i=0; i<NUM_ENTRIES; i++)); do
      randomize_metadata "$FILE"
    done
  done
 

  # Apply the same operations to /home/$USERNAME/.bash_history and /root/.bash_history
  BASH1="/home/$USERNAME/.bash_history"
  BASH2="/root/.bash_history"

  echo "Treating $BASH1 file"
  modify_file "$BASH1" "$NUM_ENTRIES"
  corrupt_file "$BASH1" "$TIMES"
  for ((i=0; i<NUM_ENTRIES; i++)); do
    randomize_metadata "$BASH1"
  done

  echo "Treating $BASH2 file"
  modify_file "$BASH2" "$NUM_ENTRIES"
  corrupt_file "$BASH2" "$TIMES"
  for ((i=0; i<NUM_ENTRIES; i++)); do
    randomize_metadata "$BASH2"
  done

  echo "File modification and corruption completed."
  echo;
  
  echo "Deleting all data corrupted..."
  echo
  echo "/var/log"
find "$DIR" -type f | while read -r FILE; do
  delete_file "$FILE"
  echo "File $FILE deleted."
done

echo
echo "/home/$USERNAME/.bash_history"
delete_file "$BASH1"
echo "File $BASH1 deleted."

echo "/root/.bash_history"
delete_file "$BASH2"
echo "File $BASH2 deleted."
}

linux_logs_corruptor_offline()
{
# Check if the user is root
if [ "$EUID" -ne 0 ]; then
  echo "Please run as root."
  exit 1
fi

# Function to modify a file with random information
modify_file() {
  local FILE="$1"
  local NUM_ENTRIES="$2"
  for i in $(seq 1 "$NUM_ENTRIES"); do
    ENTRY="Random entry #$i: $(shuf -i 1-1000 -n 1) - Random info: $(cat /dev/urandom | tr -dc 'A-Za-z0-9' | head -c 20)"
    echo "$ENTRY" >> "$FILE"
    sleep 0.1
  done
  echo "File $FILE modified with $NUM_ENTRIES random entries."
}

# Function to corrupt a file
corrupt_file() {
  local file="$1"
  local times="$2"
  for ((i=0; i<times; i++)); do
    head -c 100 /dev/urandom > "$file"
    dd if=/dev/zero bs=1 count=50 of="$file" conv=notrunc 2>/dev/null
    printf '\xFF' | dd of="$file" bs=1 seek=10 conv=notrunc 2>/dev/null
    echo "Random data" >> "$file"
    truncate -s 0 "$file" > "$file"
    printf '\x00\x00\x00\x00' | dd of="$file" bs=1 count=4 conv=notrunc 2>/dev/null
    random_file=$(find "$DIR" -type f | shuf -n 1)
    dd if="$random_file" of="$file" bs=1 count=50 conv=notrunc 2>/dev/null
    printf '\xAA' | dd of="$file" bs=1 seek=5 conv=notrunc 2>/dev/null
  done
  echo "File $file corrupted $times times.";
}

# Function to randomize metadata
randomize_metadata() {
  local FILE="$1"
  RANDOM_DAYS_MODIFICATION=$((RANDOM % 731 + 730))
  RANDOM_DAYS_ACCESS=$((RANDOM % 731 + 730))
  RANDOM_DATE_MODIFICATION=$(date -d "-${RANDOM_DAYS_MODIFICATION} days" +"%Y-%m-%d %H:%M:%S")
  RANDOM_DATE_ACCESS=$(date -d "-${RANDOM_DAYS_ACCESS} days" +"%Y-%m-%d %H:%M:%S")
  touch -d "$RANDOM_DATE_MODIFICATION" "$FILE"
  touch -d "$RANDOM_DATE_ACCESS" "$FILE"
}

randomize_directory_metadata() {
  local DIR="$1"
  RANDOM_DAYS_MODIFICATION=$((RANDOM % 731 + 730))
  RANDOM_DAYS_ACCESS=$((RANDOM % 731 + 730))
  RANDOM_DATE_MODIFICATION=$(date -d "-${RANDOM_DAYS_MODIFICATION} days" +"%Y-%m-%d %H:%M:%S")
  RANDOM_DATE_ACCESS=$(date -d "-${RANDOM_DAYS_ACCESS} days" +"%Y-%m-%d %H:%M:%S")
  touch -d "$RANDOM_DATE_MODIFICATION" "$DIR"
  touch -d "$RANDOM_DATE_ACCESS" "$DIR"
}

# Function to delete a file
delete_file() {
  local FILE="$1"
  rm -rf "$FILE"
}

# Prompt the user for the directory path containing files to corrupt
read -p "Enter the directory path containing files to corrupt: " DIR

# Check if the directory exists
if [ ! -d "$DIR" ]; then
  echo "The specified directory does not exist."
  exit 1
fi

# Prompt for the number of entries to generate
read -p "How many random entries would you like to generate? " NUM_ENTRIES

# Check if the number of entries is a valid number
if ! [[ "$NUM_ENTRIES" =~ ^[0-9]+$ ]]; then
  echo "Please enter a valid number."
  sleep 3
  exit 1
fi

# Prompt for the number of times to apply each corruption method
read -p "Enter the number of times to apply each corruption method: " times

# Validate that the input is a positive integer
if ! [[ "$times" =~ ^[0-9]+$ ]] || [ "$times" -le 0 ]; then
  echo "Please enter a valid positive integer."
  exit 1
fi

# Iterate over all files in the specified directory
echo
echo
echo "Inserting randomly generated data entries into files";
find "$DIR" -type f | while read -r FILE; do
    modify_file "$FILE" "$NUM_ENTRIES"
done

echo
echo "Corrupting all files in the directory";
find "$DIR" -type f | while read -r FILE; do
    corrupt_file "$FILE" "$times"
    #randomize_metadata "$FILE"  # Randomize metadata after modification and corruption
done

echo
echo "Falsifying access and changing data of all files";
find "$DIR" -type f | while read -r FILE; do
    # Randomize metadata the same number of times as entries
    for ((i=0; i<NUM_ENTRIES; i++)); do
        randomize_metadata "$FILE"  # Randomize metadata after modification and corruption
    done
    #echo "Modified metadata for $FILE: $NUM_ENTRIES times | Last modified set to $RANDOM_DATE_MODIFICATION, Last accessed set to $RANDOM_DATE_ACCESS."
    # Notify the user after randomizing metadata
    echo "Modified metadata for $FILE: $NUM_ENTRIES times | Last modified set to $RANDOM_DATE_MODIFICATION, Last accessed set to $RANDOM_DATE_ACCESS."
done

echo;
# Randomize metadata again in directories...
echo "Falsifying access and changing data of all directories";
find "$DIR" -type d | while read -r DIRECTORY; do
    # Randomize metadata the same number of times as entries
    for ((i=0; i<NUM_ENTRIES; i++)); do
        randomize_directory_metadata "$DIRECTORY"  # Randomize metadata for directories
    done
    # Notify the user after randomizing metadata
    echo "Modified metadata for Directory $DIRECTORY: $NUM_ENTRIES times."
done


echo
find "$DIR" -type f | while read -r FILE; do
  delete_file "$FILE"
  echo "File $FILE deleted."
done



echo "File modification, corruption, and deletion completed."
echo
}



#menu to select operation!

echo;
while true; do
  echo "Linux Logs Corruptor Menu"
  echo "---------------------------"
  echo "1. Corrupt logs in /var/log and local and root Bash history"
  echo "2. Corrupt and delete logs offline with Linux Live USB"
  echo "3. Exit"
  read -p "Enter your choice: " choice

  case $choice in
    1)
      echo "Running linux_logs_corruptor..."
      # chamada para o método linux_logs_corruptor
      linux_logs_corruptor_online
      ;;
    2)    
      echo "Running linux_logs_corruptor_deleter..."
      # chamada para o método linux_logs_corruptor_deleter
      linux_logs_corruptor_offline
      ;;
    3)
      echo "Exiting..."
      exit 0
      ;;
    *)
      echo "Invalid choice. Please try again."
      ;;
  esac
done
}


#---------------------------------------------------------------------------
#---------------------------------------------------------------------------
#---------------------------------------------------------------------------
linux_logs_shred_master()
{
# Function to display usage information
usage() {
    echo "Shred options in use:"
    echo "  -v : Verbose mode, shows progress."
    echo "  -z : Add a final overwrite with zeros to hide shredding."
    echo "  -u : Remove the file after overwriting."
    echo "Example: $0"
    exit 1
}

# Check if shred is installed
if ! command -v shred &> /dev/null; then
    echo "Error: shred is not installed. Please install it and try again."
    echo "You can install it using: sudo apt install shred -y"
    sleep 4
    exit 1
fi

# Warning about using shred on HDDs
echo "Warning: This program should only be used on HDDs to annihilate logs and metadata."
#echo "Warning: To delete swap data to prevent metadata recovery, it is only possible with a secure erase of the swap partition."
echo "Make sure you are not using SSDs or similar storage devices. Continue? (y/n)"
read -r confirm
if [[ ! "$confirm" =~ ^[yY]$ ]]; then
    echo "Operation canceled."
    sleep 3
    exit 1
fi

# Prompt user for the number of passes
echo "Enter the number of passes for shredding (positive integer):"
read -r passes

# Validate the number of passes
if ! [[ "$passes" =~ ^[0-9]+$ ]] || [ "$passes" -le 0 ]; then
    echo "Error: The number of passes must be a positive integer."
    exit 1
fi

# Shred files in /var/log/
echo "Shredding all files in /var/log/ with $passes passes."
find /var/log/ -type f -exec shred -vzu -n "$passes" {} \;

# Shred user bash history
echo "Shredding user bash history: ~/.bash_history with $passes passes."
shred -vzu -n "$passes" ~/.bash_history

# Shred root bash history
echo "Shredding root bash history: /root/.bash_history with $passes passes."
shred -vzu -n "$passes" /root/.bash_history

echo "Shredding completed."
echo
}


#---------------------------------------------------------------------------
#---------------------------------------------------------------------------
#---------------------------------------------------------------------------
describe_options() {
echo "Why and for what use Annihilator?";
echo "1. Corruptor: Corrupts all files in One Directory recursively as many times as you want to avoid forensic analysis and recovery. Changing metadata also modifies access data."
echo "2. Corruptor File: Corrupts One Specific File as many times as you want to avoid forensic analysis and recovery. Changing metadata also modifies access data."
echo "3. Log Storm: Adds entries randomly inside all files in a directory for basic sabotage forensic carving."
echo "4. Renamer Oblivion: Changes the name and extension of all files in One Directory as many times as you want to avoid forensic analysis and recovery."
echo "5. Renamer Oblivion File: Changes the name and extension of One Specific File as many times as you want to avoid forensic analysis and recovery."
echo "6. Renamer Oblivion Dir: Changes the name and extension of One Directory as many times as you want to avoid forensic analysis and recovery."
echo "7. Mega Ultra Corruptor: Performs the following actions in order: data entry modifications, file corruption, changes to access and modification metadata, randomly renames the file name and extension, and finally, changes to access and modification metadata again according to the number of entries you want!"
echo "8. Metadata Changer File: Changes access permissions and modifies data randomly for One Specific File to further obscure file contents and access history."
echo "9. Metadata Changer Dir: Changes access permissions and modifies data randomly for all files in a specified directory to further obscure file contents and access history."
echo "10. Shred a single or all files recursively in a directory (secure erase | only on HDD)"
echo "11. GOST R 50739-95 & Gutmann Secure Erase for partition or HDD (/dev/zero & /dev/urandom | only on HDD)"
echo "12. Linux Logs Corruptor (anti-carving and anti-forensic investigation for Linux log files on SSDs)"
echo "13. Linux Logs Shred (securely erase log files in Linux | only on HDD)"
echo "14. What does each option do, why, and for what use is this program?"
echo "15. Exit: Exits the program."
echo "Why attack files with corruption and changing names and extensions?";
echo "SSDs and other similar devices do not allow secure erase methods such as zero fill and urandom fill.";
echo "In SSDs and similar devices, you can corrupt files, /var/logs, and other directories and files to prevent forensic attacks from recovering data, as secure erase does not work on SSDs and similar devices.";
echo "For strengthening secure erasing! First:";
echo "a - Use Corruptor (1 or 2) on files or directories against all files to delete!";
echo "b - Use Renamer (option 3 or 4, and then in the main directory with 5 just for directories).";
echo "c - Again, use Corruptor against this directory or file.";
echo "d - Do not delete, but erase using shred or zero fill and urandom fill methods for secure erasing!";
echo "If secure erase fails, the corruption of the files will protect you against enemies who want to recover your data!";
echo "In SSDs, your secure erase is inefficient, but you can corrupt files and erase normally, and these corruption attacks can protect you from data recovery and forensic attacks from enemies!";
echo "You can select 100, 200, up to 10,000 or even more entries. The more entries, the harder it will be for forensic attackers!";
echo "Recommendation: use at least 10,000 entries!";
echo "Annihilator for secure erasing..."
echo "Options 10, 11 and 13 securely erase data and partitions on HDDs. It is not reliable for flash memory, SSDs, eMMC, UFS, NAND, SD, and NVMe."
echo "Developer -> leandroibov";
echo "";

}


#starting...
echo;
if [ "$EUID" -ne 0 ]; then
    echo "Please run as root"
    exit 1
fi
echo;

echo "--------------------------------------------------------------------------------------------------------------------------------------";
#echo "Annihilator"
echo "   _____                .__.__    .__.__          __                "
echo "  /  _  \\   ____   ____ |__|  |__ |__|  | _____ _/  |_  ___________ "
echo " /  /_\\  \\ /    \\ /    \\|  |  |  \\|  |  | \\__  \\   __\\/  _ \\_  __ \\"
echo "/    |    \\   |  \\   |  \\  |   Y  \\  |  |__/ __ \\|  | (  <_> )  | \\/"
echo "\\____|__  /___|  /___|  /__|___|  /__|____(____  /__|  \\____/|__|   "
echo "        \\/     \\/     \\/        \\/             \\/                    "

echo "Protect your data by using corruption to prevent forensic recovery in flash memory, SSDs, eMMC, UFS, NAND, SD, and NVMe."
echo "These storage media are not reliable for secure erase!"
echo "Options 10,11 and 13 secure erase data and partitions in HDD. It is not reliable in flash memory, SSDs, eMMC, UFS, NAND, SD, and NVMe"
echo "--------------------------------------------------------------------------------------------------------------------------------------";
echo;
while true; do
echo "Annihilator Options Menu"
echo "1. Corruptor: All Files in One Directory"
echo "2. Corruptor File: One Specific File"
echo "3. Log Storm (Adding entries randomly inside all files in a directory for basic sabotage forensic carving)"
echo "4. Renamer Oblivion (Changing name and extension of all files in One Directory)"
echo "5. Renamer Oblivion File (Changing name and extension of One Specific File)"
echo "6. Renamer Oblivion Dir (Changing name and extension of One Directory)"
echo "7. Mega Ultra Corruption (Do for directories steps 1, 3, 9, 4 and change metadata again (9))"
echo "8. Metadata Changer File (Change access and change data randomly of the file)"
echo "9. Metadata Changer Dir (Change access and change data randomly of all files in some directory)"
echo "10. Shred a single or all files recursively in a directory (secure erase | only on HDD)"
echo "11. GOST R 50739-95 & Gutmann Secure Erase for partition or HDD (/dev/zero & /dev/urandom | only on HDD)"
echo "12. Linux Logs Corruptor (anti-carving and anti-forensic investigation for Linux log files on SSDs)"
echo "13. Linux Logs Shred (securely erase log files in Linux | only on HDD)"
echo "14. What does each option do, why, and for what use is this program?"
echo "15. Exit"


  read -p "Choose an option: " option
  echo

  case $option in
    1)
      
      corruptor
      ;;
    2)
      
      corruptor_files
      ;;
    3)
      
      logstorm
      ;;
    4)
     
      renamer_oblivion
      ;;
    5)
      
      renamer_oblivion_file
      ;;
    6)
     
      renamer_oblivion_dir
      ;;
    7)
      mega_ultra_corruptor
      ;;
    8)
      metadata_changer_file
      ;;
    9)
      metadata_changer_dir
      ;;
    10)
      shred_dir
      ;;
    11)
      zero_urandom_fill
      ;;
    12)
      linux_logs_corruptor_master
      ;;
    13)
      linux_logs_shred_master
      ;;
    14)
      describe_options
      ;;
    15)
      # Exit the program
      exit 0
      ;;
    *)
      echo "Invalid option. Please choose a valid option."
      ;;
  esac
done
