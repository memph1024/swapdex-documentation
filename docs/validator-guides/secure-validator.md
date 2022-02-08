# **SECURE YOUR VALIDATOR**

Validators in a Proof of Stake network are responsible for keeping the network in consensus and verifying state transitions. As the number of validators is limited, validators in the set are responsible for being online and faithfully executing their tasks.

This primarily means that validators:
- Must be highly available.
- Must stay honest
- Must have infrastructure that protects the validator's signing keys so that an attacker cannot take control and commit slashable behavior.

---

## **High Availability**
A typical technique that promises high availability is to the setup multiple replicas of a node on different servers. The idea is if the active validator's server goes down, many exact copies are ready and can pick up the work with zero downtime. Although such a setup may seem attractive at first glance, they are dangerous if not set up perfectly. This is because all inactive nodes, waiting on the sideline, must potentially use the exact same session key to continue the exact same work of the failed active validator. However, you should always isolate the session keys used by you to just a single node. Replicating session keys across multiple nodes could lead to equivocation slashes. Those slashes can make you lose significant amounts of your staked funds.

The good news is that 100% uptime of your validator is not needed, as it has some buffer within eras to go offline for a bit of while and upgrade. For this reason, we advise that you only attempt a high availability setup if you're confident you know exactly what you're doing.

Many expert validators on other substrate based networkd have made mistakes in the past due to bad handling of their session keys.

Remember, even if your validator goes offline for some time, the offline slash is much more forgiving than the equivocation slash.

Read more on slashes in the staking deep-dive.

---

## **Key Management**
The keys that are of primary concern for validator infrastructure are the Session keys. These keys sign messages related to consensus. Although Session keys are not account keys and therefore cannot transfer funds, an attacker could use them to commit slashable behavior.

Session keys are generated inside the node via RPC call.

Session keys should be generated and kept within your client. When you generate new Session keys, you must submit an extrinsic (a Session certificate) from your Controller key telling the chain your new Session keys.

!!! Note
    Session keys can also be generated outside the client and inserted into the client's keystore via RPC. For most users, we recommend using the key generation functionality within the client.

---

## **Monitoring Tools**

- <a href="https://telemetry.polkadot.io/#list/0x4959f8d87d40d9ef516459ff177111bb03d875e5a7ed69282f6b689a707b69f5" target="_blank"> Telemetry UI </a> This tracks your node details including the version you are running, block height, CPU & memory usage, block propagation time, etc.
- Prometheus-based monitoring stack, including Grafana for dashboards and log aggregation. It includes alerting, querying, visualization, and monitoring features and works for both cloud and on-premise systems. The data from substrate-telemetry can be made available to Prometheus through exporters like [this](https://github.com/w3f/substrate-telemetry-exporter).

---

## **Linux Best Practices**
- Never use the root user.
- Always update the security patches for your OS.
- Enable and set up a firewall.
- Never allow password-based SSH, only use key-based access.
- Disable non-essential SSH subsystems (banner, motd, scp, X11 forwarding) and harden your SSH configuration (<a href="https://stribika.github.io/2015/01/04/secure-secure-shell.html" target="_blank"> reasonable guide to begin with </a>).
- Back up your storage regularly.

---

## **Conclusions**
- Validators should only run the Kusari/SwapDex binary, and they should not listen on any port other than the configured p2p port.

- Validators should run on bare-metal machines, as opposed to VMs. This will prevent some of the availability issues with cloud providers, along with potential attacks from other VMs on the same hardware. The provisioning of the validator machine should be automated and defined in code. This code should be kept in private version control, reviewed, audited, and tested.

- Session keys should be generated and provided in a secure way.

- Kusari/SwapDex should be started at boot and restarted if stopped for any reason.

- Kusari/SwapDex should run as a non-root user.

---

## **Recommended Reads**
- <a href="https://kb.certus.one/" target="_blank"> Certus One's Knowledge Base </a>

<p align=right> Written by Masterdubs & Petar </p>