**Brief Recap on VMs:**
Virtual machines are one instance of this trend. Generally, with a virtual machine, guest operating systems and applications run in an environment that appears to them to be native hardware. This environment behaves toward them as native hardware would but also protects, manages, and limits them. Basically, the programs running inside a VM don't know they are in one! they think its the real hardware.. scary i know.

![[redpillFinalBoss.png]]

---

### 1. Why Use a VM?

1. **Isolation**: ones fault doesn't effect the others.
2. **Testing & Development:** you can go crazy! no limits.
3. **Snapshot & Recovery:** take a snapshot (save) then trash up a VM.. download 919191 viruses then redownload the snapshot! boom all viruses are gone!
4. **Cloud Computing & Scalability**: you can host your services (websites, apps..etc) online using VMs that are part of a big computer somewhere on earth!

---

### 2. Types of VMs

1. **System Virtual Machines**
   - Acts as its own computer has its own OS.
   - You can run multiple OS on the same computer!
2. **System Virtual Machines**
   - Run a single program in an independent environment.
   - Destroy the environment after the process is done.

---

### 3. OS Support of Virtualization

T**ypes of OS:**

1. **Guest OS:** runs inside the VM (thinks its the real PC)
2. **Host OS (Hypervisor):** Manages VMs (the real PC)

**Types of Hypervisors:**

1. **Type 1 (Bare-metal)**: Runs directly on hardware (only used for computers which are meant to run solely on VMs).
2. **Type 2 (Hosted)**: Runs on top of a host OS (e.g., VirtualBox, VMware Workstation).

> [!warning] This is where my notes end (hopefully everything is covered)... feel free to search for more details

---

> 🖋️ Author: Asmaa Alazmi
