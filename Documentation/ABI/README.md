Here’s a polished **Markdown version** of the Linux Kernel ABI documentation with clear structure, highlights, and visual representations:

---

# 📘 Linux Kernel ABI Documentation

The `Documentation/ABI` directory describes the **Application Binary Interface (ABI)** between the Linux kernel and userspace. It also defines the **stability levels** of these interfaces.  

Because Linux evolves continuously, interfaces mature at different rates, and userspace programs must treat them according to their stability level.

---

## 🔒 ABI Stability Levels

There are **four levels of ABI stability**, represented by subdirectories:

| Level        | Meaning | Usage Guidance |
|--------------|---------|----------------|
| **`stable/`** ✅ | Interfaces guaranteed stable for at least **2 years**. Most (like syscalls) are expected to remain indefinitely. | Safe for all userspace programs. |
| **`testing/`** 🧪 | Interfaces considered stable but may evolve with new features. Existing functionality won’t break unless critical errors/security issues arise. | Programs can rely on them but must stay alert to changes. Developers should list their programs as *Users* for notifications. |
| **`obsolete/`** ⚠️ | Interfaces still present but scheduled for removal. Documentation explains *why* and *when*. | Avoid new usage; migrate away if possible. |
| **`removed/`** ❌ | Interfaces that have already been removed. | Historical reference only. |

---

## 📄 File Structure

Each ABI documentation file must include:

- **What:** *Short description of the interface*  
- **Date:** *Date created*  
- **KernelVersion:** *(Optional)* First kernel version where the feature appeared  
  - ⚡ Git history often provides more accurate info, so this field may be omitted  
- **Contact:** *Primary contact (e.g., mailing list)*  
- **Description:** *Detailed explanation of the interface and usage*  
- **Users:** *List of programs relying on this interface*  
  - 🔑 Especially important for `testing/` interfaces to coordinate with kernel developers and prevent breakage  

> ⚠️ **Note:**  
> - Use simple notation compatible with *ReST markup*.  
> - Files **must not** include a top‑level index like:  
>   ```  
>   ===  
>   foo  
>   ===  
>   ```

---

## 🔄 Movement Between Levels

- `stable/` → may move to `obsolete/` with proper notification.  
- `obsolete/` → may be removed after the documented grace period.  
- `testing/` → may move to `stable/` once complete.  
- `testing/` → cannot be removed directly; must first move to `obsolete/`.  
- Developers decide the initial category for new interfaces.  

---

## 🚫 Non‑ABI Elements (Never Stable)

The following **must not be relied upon by userspace**:

- **Kconfig:**  
  Do not depend on the presence/absence of any Kconfig symbol in:  
  - `/proc/config.gz`  
  - `/boot/.config`  
  - Kernel build process  

- **Kernel‑internal symbols:**  
  Do not rely on presence, absence, location, or type of kernel symbols (e.g., in `System.map` or kernel binary).  
  👉 See `Documentation/process/stable-api-nonsense.rst` for details.  

---
