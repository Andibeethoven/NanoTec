# NanoTec
Nanotechnology that needs training to save your life
SOFTWARE LICENSE AGREEMENT

This Software License Agreement (the "Agreement") is entered into by and between the original creator of the software (the "Author") and any user of the software. By using the software, you agree to the terms of this Agreement.

This Software License Agreement (the "Agreement") is entered into by and between the original creator of the software (the "Author") and any user of the software. By using the software, you agree to the terms of this Agreement.

1. LICENSE GRANT
   The Author grants you, the Licensee, a personal, non-transferable, non-exclusive, and revocable license to use the software solely for personal or commercial purposes as specified by the Author. You may not distribute, sublicense, or sell the software unless explicitly authorized by the Author in writing.

2. INTELLECTUAL PROPERTY RIGHTS
   All rights, title, and interest in and to the software, including all intellectual property rights, are and shall remain the exclusive property of the Author. This includes but is not limited to the code, designs, algorithms, and any associated documentation.

3. RESTRICTIONS
   You, the Licensee, shall not:
   a. Copy, distribute, or modify the software except as expressly authorized by the Author.
   b. Use the software for any illegal or unauthorized purposes.
   c. Reverse-engineer, decompile, or attempt to derive the source code or algorithms of the software unless explicitly permitted by law.
   d. Remove or alter any proprietary notices, labels, or markings included in the software.

4. DISCLAIMER OF WARRANTIES
   The software is provided "as is," without any warranties, express or implied, including but not limited to the implied warranties of merchantability, fitness for a particular purpose, and non-infringement. The Author does not warrant that the software will be error-free or uninterrupted.

5. LIMITATION OF LIABILITY
   In no event shall the Author be liable for any direct, indirect, incidental, special, consequential, or exemplary damages (including, but not limited to, damages for loss of profits, goodwill, or data) arising out of the use or inability to use the software, even if the Author has been advised of the possibility of such damages.

6. TERMINATION
   This license is effective until terminated. The Author may terminate this Agreement at any time if you violate its terms. Upon termination, you must immediately cease all use of the software and destroy any copies in your possession.

7. GOVERNING LAW
   This Agreement shall be governed by and construed in accordance with the laws of [Your Country/State], without regard to its conflict of laws principles.

8. AUTHORIZED USE AND SALE
   Only the Author is authorized to sell or distribute this software. Any unauthorized use, sale, or distribution of the software is strictly prohibited and will be subject to legal action.

9. ENTIRE AGREEMENT
   This Agreement constitutes the entire understanding between the parties concerning the subject matter and supersedes all prior agreements.

By using this software, you acknowledge that you have read, understood, and agreed to be bound by the terms of this Agreement.

Name : Travis Peter Lewis Johnston
Date: 19/04/2025


import numpy as np
import matplotlib.pyplot as plt

# === Communicating LUXCell Nanotechnology System ===
class CommunicatingLUXCell:
    def __init__(self, id, base_n2=2.0):
        self.id = id
        self.n1 = 1.0  # Refractive index of environment
        self.n2 = base_n2  # Internal refractive index
        self.energy_delivered = 0.0
        self.refraction_history = []
        self.signal_buffer = []

    def tune_refractive_index(self, tissue_phase_shift):
        self.n2 += 0.01 * np.tanh(tissue_phase_shift)

    def compute_refraction(self, angle_incident_deg):
        theta_i = np.radians(angle_incident_deg)
        try:
            theta_r = np.arcsin(self.n1 / self.n2 * np.sin(theta_i))
            return np.degrees(theta_r)
        except:
            return None

    def deliver_energy(self, intensity=1.0):
        delivered = intensity * (1 / self.n2)
        self.energy_delivered += delivered
        return delivered

    def receive_signal(self, signal):
        """Receive signals from other nanobots and adjust internal index."""
        if signal is not None:
            self.n2 += 0.005 * np.tanh(signal)

    def send_signal(self):
        """Create signal based on current energy state."""
        return np.tanh(self.energy_delivered / 10)

    def operate(self, tissue_phase_sequence, neighbors=None, angle_incident_deg=30):
        for phase_shift in tissue_phase_sequence:
            self.tune_refractive_index(phase_shift)
            theta = self.compute_refraction(angle_incident_deg)
            if theta is not None:
                self.refraction_history.append((self.n2, theta))
                self.deliver_energy()

            if neighbors:
                for neighbor in neighbors:
                    neighbor.receive_signal(self.send_signal())

        return self.energy_delivered

    def visualize_operation(self):
        if not self.refraction_history:
            print("No refraction history to display.")
            return
        n2_vals, angles = zip(*self.refraction_history)
        plt.figure(figsize=(10, 6))
        plt.plot(n2_vals, angles, marker="o", label=f"Nano-{self.id}")
        plt.title(f"Nano-{self.id} Refraction Angle vs Internal Refractive Index")
        plt.xlabel("Internal Refractive Index (n2)")
        plt.ylabel("Refracted Angle (degrees)")
        plt.grid(True)
        plt.legend()
        plt.show()

# === Example Swarm Simulation ===
np.random.seed(42)
tissue_phase_sequence = np.random.uniform(-1, 1, 50)

# Create a small swarm of 3 nanobots
nano1 = CommunicatingLUXCell(id=1)
nano2 = CommunicatingLUXCell(id=2)
nano3 = CommunicatingLUXCell(id=3)

# Assign neighbors for communication
nano1_neighbors = [nano2, nano3]
nano2_neighbors = [nano1, nano3]
nano3_neighbors = [nano1, nano2]

# Run each nanobot with shared signaling
nano1.operate(tissue_phase_sequence, neighbors=nano1_neighbors)
nano2.operate(tissue_phase_sequence, neighbors=nano2_neighbors)
nano3.operate(tissue_phase_sequence, neighbors=nano3_neighbors)

# Visualize each nanobot's behavior
nano1.visualize_operation()
nano2.visualize_operation()
nano3.visualize_operation()
