# OSI Model 

The OSI (Open Systems Interconnection) model describes how data moves through a network using seven logical layers, from physical signals up to user-facing applications. 

## The 7 layers (top → bottom)

1. **Application (Layer 7)** – Interfaces with user applications and provides network services such as HTTP, FTP, SMTP, and DNS. 
2. **Presentation (Layer 6)** – Translates, encrypts, and compresses data so it is in a usable format for the application layer. 
3. **Session (Layer 5)** – Sets up, manages, and tears down communication sessions between endpoints. 
4. **Transport (Layer 4)** – Provides end‑to‑end delivery, reliability, and flow control using protocols like TCP and UDP. 
5. **Network (Layer 3)** – Handles logical addressing and routing of packets between different networks, typically using IP. 
6. **Data Link (Layer 2)** – Moves frames between directly connected nodes and uses MAC addresses for local delivery. 
7. **Physical (Layer 1)** – Transmits raw bits as electrical, optical, or radio signals over the physical medium.

## Why it matters

The OSI model is mainly a **reference model**: it helps understand, design, and troubleshoot networks by separating concerns into clear layers.
