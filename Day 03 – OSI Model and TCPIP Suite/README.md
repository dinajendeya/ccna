
## Day 03 – OSI Model & TCP/IP Suite

### What Is a Networking Model?
A structured framework that categorizes protocols and standards, defining logical rules for how devices and software communicate.

### OSI Model — Overview
- **Open Systems Interconnection (OSI)** conceptual model created by ISO.
- Standardizes network functions into **7 layers** that operate together end‑to‑end.
- Key processes:
  - **Encapsulation**: data moves **down** the stack, headers/trailers added.
  - **De‑encapsulation**: data moves **up** the stack, headers/trailers removed.
  - **Same‑layer interaction**: peer processes at the **same layer** on different hosts communicate using agreed protocols.

### OSI Layers and Core Responsibilities
#### 7) Application
- Closest to the end user; interfaces with applications.
- Examples of functions: identify communication partners, synchronize application communication.
- Example protocols: **HTTP/HTTPS**.

#### 6) Presentation
- Translates, formats, and prepares application data for network transmission (e.g., data representation, character encoding, compression/encryption formatting).

#### 5) Session
- Establishes, manages, and terminates **sessions** between local and remote applications (dialog control, session checkpoints).

> Note: Network engineers typically work less with Layers **5–7**; application developers interact more with these layers.

#### 4) Transport
- **Host‑to‑host** communication; segmentation and reassembly.
- Breaks large data into **segments** to improve reliability and efficiency.
- Adds a Layer 4 header, producing a **Segment**.

#### 3) Network
- **Inter‑network** connectivity, logical addressing, and **path selection**.
- Adds a Layer 3 header, producing a **Packet**.
- **Routers** operate at this layer.

#### 2) Data Link
- **Node‑to‑node** delivery over a physical medium; framing, media access, and error detection (and possibly correction).
- **Layer 2 addressing** (distinct from L3).
- Adds **L2 header and trailer**, producing a **Frame**.
- **Switches** operate at this layer.

#### 1) Physical
- Electrical/optical/radio signaling, connectors, cabling specs, and bit transmission.
- Converts bits to physical signals and vice versa.

### OSI PDUs (Protocol Data Units)
- **Layers 7–5:** **Data**
- **Layer 4:** **Segment** (L4 header added)
- **Layer 3:** **Packet** (L3 header added)
- **Layer 2:** **Frame** (L2 header + trailer)
- **Layer 1:** **Bits** (0s and 1s on the medium)

### TCP/IP Suite — Overview
- Practical, **in‑use** model for the Internet and modern networks; developed via DARPA.
- Similar goals to OSI but with **fewer layers** and specific protocol families (e.g., **TCP**, **IP**).
- OSI remains valuable for conceptual clarity; TCP/IP defines the protocols implemented in real networks.

### Layer Interaction Concepts
- **Adjacent‑layer interactions**: a layer provides services to the layer **above** and uses services of the layer **below** on the **same** device (e.g., Layer 4 accepting data from Layer 5–7 and adding an L4 header).
- **Same‑layer interactions**: peers at the **same layer** on **different** devices communicate following their layer’s protocol rules (e.g., an Application layer client and server exchanging HTTP data).
