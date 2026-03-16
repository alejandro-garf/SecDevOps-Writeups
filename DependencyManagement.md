# TryHackMe Room Writeup — Dependencies & Supply Chain Security

---

## What are Dependencies?

A single import can bring in far more code than it appears — with a conservative estimate of 250 lines of code per import, one line of code can execute roughly **8,000 lines of dependency code**.

**Dependency management** is the process of managing your dependencies throughout your SDLC process and DevOps pipeline. Most large organisations use dedicated tools for this, such as **JFrog Artifactory**.

---

## Internal vs External Dependencies

| Type | Description | Example |
|---|---|---|
| **External** | Developed and maintained outside of your organisation. | jQuery |
| **Internal** | Developed by someone within your organisation, typically to standardise an internal process and avoid reinventing the wheel across multiple applications. | An in-house authentication library |

---

## Securing External Dependencies

External dependencies present a significant attack surface. It is often far more lucrative for an attacker to compromise a dependency used by an application than to attack the application directly.

**Practical Steps Performed:**
1. Downloaded and edited the `auth.js` file on the AttackBox.
2. Overwrote the original `auth.js` with the modified version.
3. Started a listener to intercept credentials.

**Question Answers:**
- **MageCart** is notorious for supply chain attacks.
- Intercepted password: `supersecretpassword12345@`
- Flag: `THM{Supply.Chain.Attacks.Are.Super.Powerful}`

---

## Securing Internal Dependencies

When using internal dependencies, our organisation is responsible for **all aspects** of their security and management.

One notable attack that can be performed against dependency managers is **Dependency Confusion**.

---

## Theory of Dependency Confusion

- An attacker can create a **race condition** that causes a malicious dependency to be used instead of the intended internal one.
- All an attacker needs to stage a dependency confusion attack is the **name of one of your internal dependencies**.

### Dependency Confusion vs Typosquatting

| Attack | Method |
|---|---|
| **Typosquatting** | Relies on developers mistyping the name of a package they wish to install. |
| **Dependency Confusion** | Relies on manipulating the **version number** to cause a package manager to pull a malicious external package over the legitimate internal one. |
