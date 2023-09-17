import random

# Application Layer
def application_layer(message):
    print("Application Layer: Message received -", message)
    return message

# Presentation Layer
def presentation_layer(data):
    encrypted_data = data[::-1]  # Simple encryption (reversal)
    print("Presentation Layer: Data encrypted -", encrypted_data)
    return encrypted_data

def presentation_layer_reverse(data):
    decrypted_data = data[::-1]  # Simple decryption (reversal)
    print("Presentation Layer: Data decrypted -", decrypted_data)
    return decrypted_data

# Session Layer
def session_layer(data):
    print("Session Layer: Data session established")
    return data

# Transport Layer
def transport_layer(data):
    packet = {
        "data": data,
        "checksum": random.randint(0, 1)  # Simulate checksum (0 for success, 1 for error)
    }
    if packet["checksum"] == 0:
        print("Transport Layer: Packet sent successfully -", packet)
    else:
        print("Transport Layer: Packet sent with error -", packet)
    return packet

# Network Layer
def network_layer(packet):
    print("Network Layer: Data routed through the network")
    return packet

# Data Link Layer
def data_link_layer(packet):
    frame = {
        "packet": packet,
        "parity_bit": random.choice(["even", "odd"])  # Simulate parity bit
    }
    print("Data Link Layer: Frame created with parity bit -", frame)
    return frame

def data_link_layer_verify(frame):
    if random.randint(0, 1) == 0:  # Simulate frame received without errors
        print("Data Link Layer: Frame received successfully -", frame)
        return frame["packet"]
    else:
        print("Data Link Layer: Frame received with errors -", frame)
        return None

# Physical Layer
def physical_layer(frame):
    print("Physical Layer: Frame transmitted over the physical medium")
    return frame

def main():
    message = "Hello, OSI Model!"

    # Sender's perspective
    print("Sender's Perspective:")
    print("---------------------")
    data = application_layer(message)
    data = presentation_layer(data)
    data = session_layer(data)
    packet = transport_layer(data)
    packet = network_layer(packet)
    frame = data_link_layer(packet)
    frame = physical_layer(frame)

    # Receiver's perspective
    print("\nReceiver's Perspective:")
    print("-----------------------")
    frame_received = physical_layer(frame)
    packet_received = data_link_layer_verify(frame_received)
    if packet_received:
        packet_received = network_layer(packet_received)
        packet_received = transport_layer(packet_received)
        data_received = session_layer(packet_received["data"])
        data_received = presentation_layer_reverse(data_received)
        message_received = application_layer(data_received)
        print("\nMessage Received:", message_received)

if __name__ == "__main__":
    main()
