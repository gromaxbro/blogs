# OSI Model (Quick Notes)

The OSI (Open Systems Interconnection) model describes how data moves through a network using seven logical layers, from physical signals up to user-facing applications. [web:291][web:292]

## The 7 layers (top → bottom)

1. **Application (Layer 7)** – Interfaces with user applications and provides network services such as HTTP, FTP, SMTP, and DNS. [web:292][web:293]  
2. **Presentation (Layer 6)** – Translates, encrypts, and compresses data so it is in a usable format for the application layer. [web:291][web:293]  
3. **Session (Layer 5)** – Sets up, manages, and tears down communication sessions between endpoints. [web:291][web:292]  
4. **Transport (Layer 4)** – Provides end‑to‑end delivery, reliability, and flow control using protocols like TCP and UDP. [web:291][web:293]  
5. **Network (Layer 3)** – Handles logical addressing and routing of packets between different networks, typically using IP. [web:291][web:294]  
6. **Data Link (Layer 2)** – Moves frames between directly connected nodes and uses MAC addresses for local delivery. [web:291][web:294]  
7. **Physical (Layer 1)** – Transmits raw bits as electrical, optical, or radio signals over the physical medium. [web:291][web:294]

## Why it matters

The OSI model is mainly a **reference model**: it helps understand, design, and troubleshoot networks by separating concerns into clear layers. [web:291][web:293]
