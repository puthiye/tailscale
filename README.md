# how tailscale works

Each tailscale client talks to what we call a “coordination server” (in our case, login.tailscale.com)—essentially, a shared drop box for public keys.

Here’s what happens:

- Each node generates a random public/private keypair for itself, and associates the public key with its identity (see login, below).
- The node contacts the coordination server and leaves its public key and a note about where that node can currently be found, and what domain it’s in.
- The node downloads a list of public keys and addresses in its domain, which have been left on the coordination server by other nodes.
- The node configures its WireGuard instance with the appropriate set of public keys.
- 
Note that the private key never, ever leaves its node. This is important because the private key is the only thing that could potentially be used to impersonate that node when negotiating a WireGuard session. As a result, only that node can encrypt packets addressed from itself, or decrypt packets addressed to itself. It’s important to keep that in mind: Tailscale node connections are end-to-end encrypted (a concept called “zero trust networking”).
