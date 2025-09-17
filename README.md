# Template Wizard for Nagios XI

This is a working template wizard for Nagios XI. Use it as a base to build your own configuration wizards.

---

## template Specifics

In this template, I use specifics like template_wiz for the name, check_example.py for the plugin, and template_wiz.png for the image, you should change these to be more specific to your wizard

## Notes for Making a Wizard

1. The folder and the `.inc.php` file must have the same name.  
   Example: folder name = `template_wiz`, file name = `template_wiz.inc.php`

2. `config.xml` must be updated to match your wizard specifics.

3. The main file (`template_wiz.inc.php`) must include the big switch case that handles all stages (line 32 → 155).

4. The default file structure layout in this template is known to work.  
   (`install.sh` seemingly optional?)

---


## 📂 File Structure

```
template_wiz/
├── config.xml
├── template_wiz.inc.php
├── steps/
│   ├── step1.php
│   └── step2.php
├── plugins/
│   └── check_example.py
├── logos/
│   └── template_wiz.png
└── README.md
```

---


## Uploading the Wizard

### Option 1: Web UI
Go to **Admin → System Extensions → Configuration Wizards** and upload your wizard `.zip`.

### Option 2: Manual Copy
Copy the wizard into Nagios XI and set permissions:

```bash
sudo cp -r template_wiz /usr/local/nagiosxi/html/includes/configwizards/
sudo chown -R nagios:nagios /usr/local/nagiosxi/html/includes/configwizards/template_wiz
sudo chmod -R 755 /usr/local/nagiosxi/html/includes/configwizards/template_wiz
```

---

## Installing the Plugin

Copy your plugin into the Nagios plugins directory and set permissions:

```bash
sudo cp template_wiz/plugins/check_example.py /usr/local/nagios/libexec/
sudo chown nagios:nagios /usr/local/nagios/libexec/check_example.py
sudo chmod 755 /usr/local/nagios/libexec/check_example.py
```

### Troubleshooting Plugins
- **Error 127 (plugin not found):**
  ```bash
  cd /usr/local/nagios/libexec
  ./check_example.py
  ```
- **`python3\R` error (`weird` line endings):**
  ```bash
  dos2unix /usr/local/nagios/libexec/check_example.py
  ```

---

## Adding the Logo (if you uploaded wiz via command line)

Wizard logos must be placed here:

```
/usr/local/nagiosxi/html/includes/components/nagioscore/ui/images/logos/
```

Example:

```bash
sudo cp template_wiz/logos/template_wiz.png /usr/local/nagiosxi/html/includes/components/nagioscore/ui/images/logos/
sudo chown nagios:nagios /usr/local/nagiosxi/html/includes/components/nagioscore/ui/images/logos/template_wiz.png
sudo chmod 644 /usr/local/nagiosxi/html/includes/components/nagioscore/ui/images/logos/template_wiz.png
```

---

## Final Steps

1. Verify wizard, plugin, and logo permissions:
   - Wizard folder: `nagios:nagios`, `755`
   - Plugin: `nagios:nagios`, `755`
   - Logo: `nagios:nagios`, `644`
2. Restart Nagios XI services if needed:
   ```bash
   service nagios restart
   service httpd restart
   ```
3. Open **Configure → Configuration Wizards** in the web UI.  
   Your new wizard should now appear in the list.

 You can now use your custom wizard in Nagios XI.
