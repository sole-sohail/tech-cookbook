# How to Completely Uninstall Ollama on Linux

This article provides a clear, step-by-step process to uninstall **Ollama** from a Linux system. This includes stopping services, removing binaries, cleaning up residual files, and user accounts to ensure a complete and clean removal.

---

## Step 1: Stop and Disable the Ollama Service

To safely uninstall Ollama, first stop and disable the running service:

```bash
sudo systemctl stop ollama
sudo systemctl disable ollama
```

---

## Step 2: Remove the Ollama Service File

Remove the service file from your systemd directory to prevent the service from restarting in the future:

```bash
sudo rm /etc/systemd/system/ollama.service
```

---

## Step 3: Delete the Ollama Binary

Remove the Ollama binary from your system. Use the command below, which identifies and deletes the binary regardless of its installation path (`/usr/local/bin`, `/usr/bin`, or `/bin`):

```bash
sudo rm $(which ollama)
```

---

## Step 4: Clean Up Remaining Files and User Accounts

Delete all remaining Ollama-related files, directories (including downloaded models), and user/group accounts:

```bash
sudo rm -r /usr/share/ollama
sudo userdel ollama
sudo groupdel ollama
```

---

## Step 5: Verification

Verify that Ollama is fully uninstalled by checking if any related services are still listed:

```bash
systemctl list-units --type=service | grep ollama
```

If there is no output, Ollama has been successfully removed.

---

## Conclusion

Following these steps will completely remove Ollama from your Linux system, ensuring there are no residual components or potential conflicts with future installations.

