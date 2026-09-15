# Automatic Generator Changeover Using Magnetic Contactor

A hands-on electrical engineering project that automatically transfers household load between Main Electricity Supply and Generator Supply using a magnetic contactor.

---

## 🏠 Hands-on Practical Implementation

This is a **real-world project that I personally designed, installed, and tested at my own home**. It was not a simulation or a theoretical exercise — the contactor, wiring, and load transfer logic described here were physically built and verified under actual household electrical conditions.

> ⚠️ This project reflects a personal, hands-on learning implementation. It is **not** a certified Automatic Transfer Switch (ATS), and it has not undergone professional certification or third-party safety approval. Anyone attempting a similar build should follow local electrical codes and, where required, consult or hire a licensed electrician.

---

## 📌 Project Overview

Manually switching a household between Main Supply and Generator Supply during a power outage is inconvenient and easy to get wrong. This project uses a **Magnetic Contactor** to automate that switch:

- When the **Generator is OFF**, the load runs on the **Main Supply**.
- When the **Generator is turned ON**, the load automatically transfers to the **Generator Supply**.
- When the **Generator is turned OFF again**, the load automatically returns to the **Main Supply**.

The core idea is simple: use the generator's own output to energize the contactor coil, so the contactor's mechanical contacts do the switching without any manual intervention.

---

## ⚙️ Working Principle

The contactor has a coil, one **NC (Normally Closed)** contact set, and one **NO (Normally Open)** contact set. The generator supply itself is used to drive the coil.

| State | Coil Status | Active Contact | Load Powered By |
|---|---|---|---|
| Generator OFF | De-energized | NC (closed) | Main Supply |
| Generator ON | Energized | NO (closed) | Generator Supply |
| Generator OFF again | De-energized | NC (closed) | Main Supply |

**Step-by-step logic:**

1. **Generator OFF**
   - Contactor coil is de-energized (no supply from the generator line).
   - The NC contact stays in its natural closed position.
   - Load receives power from the **Main Supply**.

2. **Generator ON**
   - The generator's output line energizes the contactor coil.
   - The contactor's armature is pulled in.
   - The NO contact closes, and the NC contact opens.
   - Load transfers to the **Generator Supply**.

3. **Generator OFF again**
   - The coil loses its power source and de-energizes.
   - The contactor returns to its normal (unenergized) state.
   - Load automatically returns to the **Main Supply**.

This gives a simple, automatic changeover with no manual switching required at the moment of transfer.

---

## 🔌 Basic Connection / Logic Diagram

```
                     ┌────────────────────────┐
   MAIN SUPPLY ──────┤  NC Contact             │
                     │                         ├──────► LOAD
   GENERATOR ────┬───┤  NO Contact             │
   SUPPLY         │  └────────────────────────┘
                  │              ▲
                  │              │
                  └──────► Contactor Coil (energized by Generator Supply)

   Generator OFF  : Coil de-energized  → NC closed → Load = MAIN SUPPLY
   Generator ON   : Coil energized     → NO closed → Load = GENERATOR SUPPLY
```

**Logic summary:**

```
IF generator_supply == ON:
    coil = ENERGIZED
    load = GENERATOR_SUPPLY   (via NO contact)
ELSE:
    coil = DE-ENERGIZED
    load = MAIN_SUPPLY        (via NC contact)
```

---

## 🧩 Main Components

- Magnetic Contactor (with one NC + one NO contact arrangement, coil rated for the generator's output voltage)
- Main Electricity Supply line
- Generator Supply line
- Household load / distribution wiring
- Protective devices (Fuse/MCB) on both supply lines
- Coil protection components (see below)
- Properly rated connecting wires and terminals

---

## 🛡️ Safety Considerations

### ⚠️ Coil Protection — Important Correction

During early wiring, it might seem tempting to use a **thin Neutral wire** on the coil circuit, on the assumption that its added resistance will "protect" the coil. **This is not a valid or safe protection method.**

Intentionally introducing resistance through undersized wire causes:

- **Voltage drop** across the wire, which can starve the coil of proper operating voltage
- **Heating** at the thin wire due to higher current density and increased I²R losses
- **Unreliable contactor operation** — the coil may under-energize, causing chattering, slow pickup, or failure to hold the armature in

Proper coil protection instead relies on components designed for that purpose:

| Protection Method | Purpose |
|---|---|
| Correctly rated Fuse / MCB | Interrupts the coil circuit safely under fault or overcurrent conditions |
| RC Snubber (for AC coils) | Suppresses voltage transients when the coil is switched, reducing arcing and electrical noise |
| MOV / Varistor | Clamps voltage spikes that could damage the coil or nearby electronics |
| Proper wire sizing | Ensures the coil receives its rated voltage/current without unintended voltage drop or heating |
| Electrical / mechanical interlocking | Prevents both supplies from being connected to the load at the same time |

Always size wiring and protective devices according to the coil's actual rated voltage and current — never rely on wire gauge as a substitute for a designed protection component.

### ⚠️ Preventing Simultaneous Supply Connection (Backfeed Prevention)

The single most important safety requirement in any changeover system is this:

> **The Main Supply and Generator Supply must never be connected to the load at the same time.**

If both supplies are ever connected together — even briefly — the results can include:

- **Backfeeding** the Main Supply line with generator power, which is extremely dangerous to utility workers performing repairs on what they believe is a de-energized line
- Damage to the generator, household appliances, or the utility's electrical infrastructure
- Fire hazard due to phase clash or short-circuit conditions

This risk is why the NC/NO contact arrangement on a single contactor is useful — the two contacts are mechanically linked and cannot both be closed at once. For any real installation, this should be reinforced with:

- **Mechanical interlocking** (physical prevention of both paths closing simultaneously)
- **Electrical interlocking** (control logic that prevents both coils/contactors from energizing together, in multi-contactor designs)
- Regular inspection of contact condition, since a welded or stuck contact could defeat the interlock

### General Electrical Safety

- ⚠️ Always disconnect and verify all supplies are de-energized before working on wiring.
- ⚠️ Generator and mains wiring should be handled with caution — treat all lines as live until proven otherwise.
- ⚠️ Use appropriately rated enclosures, insulation, and terminals for all connections.
- ⚠️ This project was implemented as a personal, educational exercise — it is not a substitute for a certified ATS panel installed by a licensed electrician, and should not be treated as one.

---

## 🎯 Learning Outcomes

- Practical understanding of contactor construction and NC/NO contact operation
- Hands-on experience wiring a real automatic load transfer circuit
- Understanding of why proper coil protection components matter, and why "thin wire resistance" is a misconception
- Awareness of backfeed hazards and the role of interlocking in changeover systems
- Bridging electrical theory (from EEE coursework) with a functioning real-world installation

---

## 🚀 Future Improvements

- Add a dedicated electrical interlocking relay/circuit as a redundant safety layer
- Add visual indicators (LEDs) showing which supply is currently active
- Integrate a microcontroller (e.g., ESP32) for logging changeover events and remote monitoring
- Add a time-delay relay to avoid rapid switching during generator startup fluctuations
- Explore upgrading to a purpose-built ATS module for improved safety certification

---

## 👨‍💻 Author

**M. Shoyab Sadik**
B.Sc. in Electrical & Electronic Engineering (EEE)
IUBAT — International University of Business Agriculture and Technology
Founder & CEO, Pico Robotics

- 🌐 Website: [shoyabsadik.github.io](https://shoyabsadik.github.io/)
- 💼 LinkedIn: [linkedin.com/in/shoyabsadik](https://www.linkedin.com/in/shoyabsadik/)
- 💻 GitHub: [github.com/shoyabsadik](https://github.com/shoyabsadik)

---

## 📜 License

This project is shared for educational purposes. Feel free to reference or learn from it, with attribution. See the [LICENSE](LICENSE) file for details, or contact the author for usage permissions.
