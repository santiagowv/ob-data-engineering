Security integrated from the ground up (<span style="color:rgb(216, 203, 251)">secure by default</span>).
# Foundational principles
- **Principle of leat privilige:** <span style="color:rgb(216, 203, 251)">Get the data we need</span>, not the data we want.
- **Defense in depth:** If <span style="color:rgb(216, 203, 251)">one lock fails</span>, there are five more behind it.
- **Automation:** <span style="color:rgb(216, 203, 251)">Manual security processes are error-prone</span> and they're not scalable.
# Data classification and discovery
- **Automated discovery:** Not about what we think we have, <span style="color:rgb(216, 203, 251)">it's about what we actually have</span>.
- **Classification framework:** <span style="color:rgb(216, 203, 251)">Practical labels</span>, not overly granular chaos.
- **Continuos monitoring:** Data changes so <span style="color:rgb(216, 203, 251)">classification should be monitored an updated</span>.
# Identity and access management
- **SSO and MFA:** Single sign-on simplifies; multi-factor secures.
- **Service accounts and secrets:** Machines get <span style="color:rgb(216, 203, 251)">temporary leases</span>.
- **RBAC and ABAC:** <span style="color:rgb(216, 203, 251)">Role and attribute</span> (time of day, location, risk score) based access control.
# Data pipeline security
- **Pipeline code security:** <span style="color:rgb(216, 203, 251)">Test pipeline code like production code</span>. Scan it, review it, and never trust it.
- **Immutable and secure deployment:** <span style="color:rgb(216, 203, 251)">Don't patch running pipelines</span>. Replace them entirely. It's cleaner, safer, and easier to undo.
# Data encryption
- **Data at rest:** If they seal the hard drives, all they get is a digital paperweight.
- **Data in transit:** <span style="color:rgb(216, 203, 251)">Like sending a message in a bottle</span>, but the bottle is bulletproof, invisible submarine.
# Logging, monitoring and anomaly detection
- **Immutable audit logging:** If it isn't logged, it didn't happen.
- **Monitoring and anomaly detection:** Detecting <span style="color:rgb(216, 203, 251)">unusual behavior</span> before it affects the system.
- **Data access analytics:** <span style="color:rgb(216, 203, 251)">Unusual access</span> from users.