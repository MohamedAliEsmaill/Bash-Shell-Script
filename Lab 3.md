# Lab 3

1. Write a script called mycase, using the case utility to checks the type of character entered by a user:
    1. Upper Case.
    2. Lower Case.
    3. Number.
    4. Nothing.

```bash
#!/bin/bash
read -p "Enter a character: " var
case $var in
    [A-Z]) echo "Upper Case";;
    [a-z]) echo "Lower Case";;
    [0-9]) echo "Number";;
    "") echo "Nothing";;
    *) echo "Invalid Input";;
esac
```

---

2. Enhanced the previous script, by checking the type of string entered by a user:

1. Upper Cases.
2. Lower Cases.
3. Numbers.
4. Mix.
5. Nothing.

```bash
#!/bin/bash
read -p "Enter a string: " var
shopt -s extglob
case $var in
    +([A-Z])) echo "Upper Cases";;
    +([a-z])) echo "Lower Cases";;
    +([0-9])) echo "Numbers";;
		+([0-9]|[a-zA-Z])) echo "Mix";;
    "") echo "Nothing";;
    *) echo "Invalid Input";;
esac
```

---

3. Write a script called mychmod using for utility to give execute permission to all files and directories in your home directory.

```bash
#!/bin/bash
cd ~
for object in *; do
    if [ -f "$object" ] || [ -d "$object" ]; then
        chmod +x "$object"
        echo "Execute permission granted for: $object"
    fi
done
```

---

4.  Write a script called mybackup using for utility to create a backup of only files in your home directory.

```bash
#!/bin/bash
backup_dir=~/backup
mkdir -p "$backup_dir"
for file in ~/; do
    if [ -f "$file" ]; then
        cp "$file" "$backup_dir/"
        echo "Backup created for: $file"
    fi
done
```

---

5. Write a script called mymail using for utility to send a mail to all users in the system. Note: write the mail body in a file called mtemplate.

```bash
#!/bin/bash
template="mtemplate"
if [ ! -f "$template" ]; then
    echo "Error: Mail template file '$template' not found."
    exit 1
fi
for user in $(cut -d: -f1 /etc/passwd); do
    mail -s "Hi from mymail script" "$user" < "$template"
    echo "Mail sent to: $user"
done
echo "Done!"
```

---

6. Write a script called chkmail to check for new mails every 10 seconds. Note: mails are saved in /var/mail/username.

```bash
#!/bin/bash
while true; do
    for user_mail in /var/mail/*; do
        username=$(basename "$user_mail")
        if mail -H "$username" 2>/dev/null | grep -q "^Status: R"; then
            echo "New mail for user: $username"
        fi
    done
    sleep 10
done
```