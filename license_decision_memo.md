# License Decision Memo

CONCEPTS: OPEN_SOURCE_LICENSE|COPYLEFT|PATENT_GRANT|TRADEMARK|CERTIFICATION_MARK|DEFENSIVE_TERMINATION

Status: Owner-approved direction (TODO 9), pending counsel review before launch. Goal: maximally open, no royalties ever, no enclosure — an x86-style compatible ecosystem where anyone builds and differentiates on top, but nobody can box in the base design or weaponize patents against other implementers.

## The three instruments

1. **Design/specs: CERN-OHL-W v2** (weakly reciprocal hardware license). Anyone may use, modify, manufacture, and sell — including proprietary products built on the design. Modifications *to the licensed design itself* must be made available under the same license when products ship. Includes an express patent grant from every contributor covering their contributions.
2. **Code (golden model, compiler, tools): Apache-2.0.** Standard, business-safe, carries a patent grant with defensive termination (§3: assert a patent claim against the work and your patent license ends).
3. **Compatibility: "VSA" certification mark.** The name/mark is reserved; calling an implementation VSA-compatible requires passing the public conformance surface (ImplSpec §10.7 clause + golden vectors). Building incompatible forks is legal — they just can't wear the badge. This, not the copyright license, is what produces the compatible-ecosystem outcome (RISC-V and OpenPOWER precedent).

## Owner patent/IP pledge
The owner holds any personal IP/patent rights in the design open: a public non-assertion covenant — the owner will never assert them against any implementation of the base design, compatible or not. Counsel drafts the covenant text (reference points: OIN license, Apache §3, RISC-V member IP policy). Adopters' own on-top innovations remain theirs to protect, so long as protection is not asserted against the base design or other implementations of it (defensive-only, enforced by the licenses' termination clauses).

## Why CERN-OHL-W (and why not)
**For:** purpose-built for hardware reference designs (covers fabrication, not just copying); the -W reciprocity blocks embrace-and-extend enclosure of the base design while leaving products proprietary; express patent grant; mature v2 text with clear definitions; compatible with the certification-mark model.
**Against:** reciprocity adds adoption friction for the most conservative corporate counsel (any modified base design must be published — some companies reflexively avoid anything reciprocal); "available" obligations trigger on conveying products, which needs explaining to adopters; less familiar to software-trained lawyers than Apache/MIT; if maximum adoption ever outweighs anti-enclosure, the fallback is CERN-OHL-P (fully permissive) at the cost of the enclosure protection.

## Enforcement model (plain English)
The license is the only thing granting rights. Breach it and the grant terminates — continued manufacture/distribution is then ordinary copyright/patent infringement with full liability. CERN-OHL v2 terminates automatically on breach with reinstatement if cured within 30 days of notice; Apache-2.0's patent grant terminates on offensive assertion. Nobody polices compliance day-to-day; the termination clause is the deterrent, and the certification mark is revocable independently for conformance failures.

## Remaining steps
Counsel review of: OHL-W fit for a docs-first release (design source = the spec docs), the non-assertion covenant text, and certification-mark registration/policy. Keep it lightweight — the ecosystem design above is settled; only the legalese is open.
