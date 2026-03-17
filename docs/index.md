# Winston HPC User Guide

<div class="ascii-logo terminal" role="region" aria-label="Winston terminal">
  <div class="terminal-bar">
    <span class="dot dot-red" aria-hidden="true"></span>
    <span class="dot dot-amber" aria-hidden="true"></span>
    <span class="dot dot-green" aria-hidden="true"></span>
    <span class="terminal-title">help@winston:~</span>
  </div>
  <div class="terminal-body" aria-live="polite">
    <div class="term-line" style="--chars: 12; --delay: 0s;">     ▘    ▗</div>
    <div class="term-line" style="--chars: 16; --delay: 0.35s;">  ▌▌▌▌▛▌▛▘▜▘▛▌▛▌</div>
    <div class="term-line" style="--chars: 16; --delay: 0.7s;">  ▚▚▘▌▌▌▄▌▐▖▙▌▌▌</div>
    <div class="term-line" style="--chars: 74; --delay: 1.05s;">  </div>
    <div class="term-line" style="--chars: 74; --delay: 1.05s;">  LSE Methodology's HPC for running CPU/GPU workloads via Slurm.</div>
    <div class="term-line" style="--chars: 74; --delay: 1.05s;">  </div>
    <div class="term-line" style="--chars: 50; --delay: 1.85s;">  Use the navigation on the left to get started.</div>
  </div>
</div>


## Quick start

1. Get your **username** and **host/IP** from the admin.
2. Connect via SSH (see **Getting started → Accessing Winston**).
3. Store code and configs in `/home`, working data in `/scratch`, shared datasets in `/data` (see **Reference → Cluster resources**).
4. Run small tests on login, submit real work via Slurm.
5. Use **Software → Miniconda** or **Software → Spack** to install tools.

!!! tip "New users"
    If you’re not sure where to start: read **Getting started → Accessing Winston**, then **Slurm jobs → Overview**.
    If you don’t yet have an account, email Tom at [t.robinson7@lse.ac.uk](mailto:t.robinson7@lse.ac.uk).

!!! note "Host details"
    For security, the Winston host/IP is shared privately during account setup.
