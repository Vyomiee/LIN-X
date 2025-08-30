# What is a shell script?
* A **shell script** is a text file containing a series of commands written for the shell to execute.
* It automates tasks that you would normally run in the terminal.
* Example: Running multiple commands, looping through files, making backups, etc.

---

## 2. Creating Your First Shell Script

1. Open a terminal and create a file:

   ```bash
   nano hello.sh
   ```

2. Add the following content:

   ```bash
   #!/bin/bash
   # This is a simple shell script

   echo "Hello, World!"
   ```

   * `#!/bin/bash` → called a **shebang**, tells the system which shell to use.
   * `echo` → prints text to the screen.
   * `#` → marks a comment.

3. Save and exit (`CTRL+O`, `CTRL+X` in nano).

4. Make it executable:

   ```bash
   chmod +x hello.sh
   ```

5. Run it:

   ```bash
   ./hello.sh
   ```

✅ Output should be:

```
Hello, World!
```

---

## 3. Variables in Shell

You can store data in variables:

```bash
#!/bin/bash

name="Vibhu"
age=37

echo "My name is $name"
echo "I am $age years old"
```

⚠️ Note: **No spaces** around `=` when assigning values.

---

## 4. Taking User Input

```bash
#!/bin/bash

echo "Enter your name:"
read username

echo "Hello, $username! Welcome to shell scripting."
```

* `read` → takes input from the user.
* `$username` → retrieves the value.

---

## 5. Conditional Statements (if-else)

```bash
#!/bin/bash

echo "Enter a number:"
read num

if [ $num -gt 10 ]
then
    echo "The number is greater than 10"
else
    echo "The number is 10 or smaller"
fi
```

* `-gt` → greater than.
* Other operators: `-lt` (less than), `-eq` (equal).

---
# File Permissions with `chmod` and `chown`

---

## 🔹 1. Understanding File Permissions in Linux

Each file/directory in Linux has:

* **Owner** → The user who created the file.
* **Group** → A group of users who may share access.
* **Others** → Everyone else.

### Permission Types

* **r** → Read (4 in numeric)
* **w** → Write (2 in numeric)
* **x** → Execute (1 in numeric)

### Permission Layout

Example from `ls -l`:

```
-rwxr-xr--
```

Breakdown:

* `-` → Regular file (`d` = directory, `l` = symlink, etc.)
* `rwx` → Owner has read, write, execute
* `r-x` → Group has read, execute
* `r--` → Others have read only

---

## 🔹 2. `chmod` – Change File Permissions

### Syntax

```bash
chmod [options] mode filename
```

Modes can be set in **numeric (octal)** or **symbolic** form.

---

### (A) Numeric (Octal) Method

Each permission is represented as a number:

* Read = 4
* Write = 2
* Execute = 1

Add them up:

* `7 = rwx`
* `6 = rw-`
* `5 = r-x`
* `4 = r--`
* `0 = ---`

#### Example:

```bash
chmod 755 script.sh
```

Meaning:

* Owner: 7 → `rwx`
* Group: 5 → `r-x`
* Others: 5 → `r-x`

---

### (B) Symbolic Method

Use `u` (user/owner), `g` (group), `o` (others), `a` (all).
Operators:

* `+` → Add permission
* `-` → Remove permission
* `=` → Assign exact permission

#### Examples:

```bash
chmod u+x script.sh     # Add execute for owner
chmod g-w notes.txt     # Remove write from group
chmod o=r file.txt      # Set others to read only
chmod a+r report.txt    # Everyone gets read access
```

---

### (C) Recursive Changes

```bash
chmod -R 755 /mydir
```

* `-R` → applies changes recursively to all files/subdirectories.

---

## 🔹 3. `chown` – Change File Ownership

### Syntax

```bash
chown [options] new_owner:new_group filename
```

### Examples:

```bash
chown vibhu file.txt           # Change owner to user 'vibhu'
chown vibhu:dev file.txt       # Change owner to 'vibhu' and group to 'dev'
chown :dev file.txt            # Change only group to 'dev'
chown -R vibhu:dev /project    # Recursive ownership change
```

---

## 🔹 4. Putting It All Together

### Example Scenario

```bash
touch project.sh
ls -l project.sh
```

Output:

```
-rw-r--r-- 1 vibhu dev 0 Aug 19 12:00 project.sh
```

Now:

```bash
chmod 700 project.sh       # Only owner has rwx
chmod u+x,g-w project.sh   # Add execute for user, remove write for group
chown root:admin project.sh # Change owner to root and group to admin
```

---

## 🔹 5. Quick Reference Table

| Numeric | Permission | Meaning      |
| ------- | ---------- | ------------ |
| 0       | ---        | No access    |
| 1       | --x        | Execute only |
| 2       | -w-        | Write only   |
| 3       | -wx        | Write + Exec |
| 4       | r--        | Read only    |
| 5       | r-x        | Read + Exec  |
| 6       | rw-        | Read + Write |
| 7       | rwx        | Full access  |

---

✅ **Key Tip**: Use **numeric for quick settings** (e.g., 755, 644) and **symbolic for fine adjustments** (`u+x`, `g-w`).

---
Practice Experiment
# Practice for creating user and groups

### 🔹 1. Create a new user

```bash
sudo useradd -m newuser
```

* `-m` → creates a home directory `/home/newuser`.

---

### 🔹 2. Create a new group

```bash
sudo groupadd newgroup
```

---

### 🔹 3. Add the user to the group

```bash
sudo usermod -aG newgroup newuser
```

* `-aG` → append user to the supplementary group (doesn’t remove existing groups).

---

### 🔹 4. Create a file (as current user, e.g. root or your login user)

```bash
touch testfile.txt
```

Check ownership:

```bash
ls -l testfile.txt
```

Example:

```
-rw-r--r-- 1 ubuntu ubuntu 0 Aug 19 14:02 testfile.txt
```

---

### 🔹 5. Assign ownership of the file to `newuser` and `newgroup`

```bash
sudo chown newuser:newgroup testfile.txt
```

---

### 🔹 6. Verify ownership

```bash
ls -l testfile.txt
```

Output:

```
-rw-r--r-- 1 newuser newgroup 0 Aug 19 14:02 testfile.txt
```

---
