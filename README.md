# Simultaneous Causality
## The Formal Structure of Coordination Failure in Safety-Critical Systems

**ERI Labs · Eric Ren · Jersey City, New Jersey · github.com/ericrenone**

*In memory of the two pilots of Jazz Aviation Flight 8646, March 23, 2026.*

---

> *"What is chance? It is the meeting of two independent causal chains."*
> — Jacques Monod, Chance and Necessity, 1970

> *"Truck One, stop, stop, stop."*
> — LaGuardia Tower, 11:40 PM ET, March 23, 2026

---

## The Technical Facts

At 11:40 PM ET on March 23, 2026, two independent causal chains intersected on Runway 4 at LaGuardia Airport.

**Chain A:** Jazz Aviation Flight 8646, a Bombardier CRJ-900 operating as Air Canada Express from Montreal, was on final approach to Runway 4. The aircraft was traveling at approximately 100 mph. The crew had received clearance to land. The approach was normal.

**Chain B:** United Airlines Flight 2384 had aborted its takeoff on a different runway after an anti-ice warning light activated and an odor was reported in the cabin. Flight attendants were feeling ill. The crew declared an emergency. A Port Authority Aircraft Rescue and Firefighting (ARFF) vehicle was dispatched to respond. The crew of Truck 1 requested permission to cross Runway 4 at taxiway Delta. ATC granted the crossing clearance.

Both chains were active simultaneously. Both chains were valid. Each was being correctly managed in isolation. Neither was conditioned on the state of the other.

The collision occurred where the two chains intersected in space and time. The cockpit was destroyed on impact. Both pilots were killed instantly. The forward section of the aircraft was severed. Forty-one people were transported to hospital.

ATC recognized the conflict and issued the stop command — "Truck One, stop, stop, stop" — but the geometry had already closed. The truck was in the runway footprint. The aircraft was committed to its landing roll. The command arrived after the causal chains had become physically irresolvable.

---

## What Happened Formally

This is not a mystery. The causal structure is transparent and it matches a precisely defined class of coordination failure.

**The independence baseline at safety-critical scale.**

The two causal chains — the landing sequence and the emergency response — were each being managed correctly within their own operational context. The landing controller cleared Flight 8646 to land. The ground controller cleared the fire truck to cross. Each action was correct conditional on its own information set. Neither action was conditioned on the information set of the other.

Formally:

```
I(landing_clearance ; crossing_clearance | runway_state) = ?
```

If this conditional mutual information is zero — if the landing clearance and the crossing clearance are statistically independent conditional on the runway's physical state — then the two clearances can conflict. One can authorize a state and the other can simultaneously authorize the incompatible state. Both clearances are locally valid. Their combination is globally catastrophic.

This is the independence baseline. G_coord = 0 between two simultaneously active causal chains operating in the same physical space.

The conditioning clause `| runway_state` is what prevents it. If the crossing clearance is conditioned on whether a landing aircraft is committed to the runway — not just whether the runway is geometrically clear at the moment of clearance, but whether the runway will be physically occupied within the time window required for the crossing — the two clearances cannot simultaneously authorize incompatible states.

The formal condition for runway safety:

```
I(landing_clearance ; crossing_clearance | runway_state_at_all_t ∈ [t_cross, t_cross + Δ]) > 0
```

Where Δ is the time window from crossing initiation to crossing completion. The conditioning must extend across the full duration of the crossing, not merely the instant of clearance. This is the temporal conditioning clause. It is the ribbon in Rovee-Collier's paradigm: absent, the two events are independent. Present, the crossing cannot be authorized if a landing aircraft will occupy the runway within the crossing window.

---

## Why the Stop Command Arrived Too Late

The air traffic control audio shows the controller issued the crossing clearance, then recognized the conflict and issued the stop command — "stop" repeated at least ten times. The controller saw the conflict. The command was correct. It was geometrically too late.

This reveals the second formal problem: the time constants of the two causal chains are mismatched.

**Chain A time constant:** a CRJ-900 on final approach at 100 mph covers approximately 147 feet per second. At a standard glide path, once the aircraft crosses the runway threshold, it requires approximately 20-30 seconds to decelerate below the speed at which evasive action is possible. The commitment point — the point past which stopping the landing roll before the intersection becomes physically impossible — occurs before the runway threshold.

**Chain B time constant:** a fire truck crossing a runway takes 10-15 seconds to clear from the far side of the pavement, depending on runway width and vehicle speed.

**The conflict window:** if the fire truck is granted clearance while a landing aircraft is within 30-45 seconds of the intersection — already past the commitment point but not yet at the intersection — the clearance has authorized a collision. The crossing cannot be completed, the landing cannot be aborted. The geometry is irresolvable before any command can change it.

The stop command arrived inside this window. The controller's recognition and response were correct but fell within the irresolvable interval. The collision was physically determined before the command was issued.

**The formal statement:**

```
Irresolvable interval = [t_commitment, t_intersection]

If:  crossing_clearance issued at t_c ∈ [t_commitment, t_intersection]
     landing_commitment at t_a < t_c
Then: no command issued at t_c < t_command < t_intersection can prevent collision

The only preventive window is: t_command < t_commitment
All ATC intervention after t_commitment is post-causal — it cannot change the outcome
```

The stop command was issued inside the irresolvable interval. The controller was responding to real information in real time. The geometry made the command irrelevant.

---

## The Monod Structure: Two Operons, One Promoter

The LaGuardia collision has the formal structure of Monod's diauxie applied to safety-critical routing:

**Two operons simultaneously induced:**
- Operon A: landing sequence protocol (induced by Flight 8646's approach)
- Operon B: emergency response protocol (induced by United 2384's declared emergency)

**One promoter:** the ATC frequency for LaGuardia Tower, which is the shared regulatory substrate through which both protocols are authorized.

**The diauxic failure:** in normal diauxie, one operon is repressed while the other is expressed — E. coli consumes glucose before lactose because the glucose pathway represses the lactose operon. The failure mode is **simultaneous induction**: both operons fully expressed at the same time, competing for the same physical resource (the runway).

The formal resolution: the crossing clearance should have acted as a **corepressor** for the landing clearance — the detection of an active emergency vehicle request on a runway with an inbound aircraft should automatically suppress the crossing authorization until the landing is complete. The two protocols need a mutual suppression mechanism: not sequential execution (which would require rejecting the emergency response), but **interlocked authorization** where each clearance is conditioned on the other's state.

This is the allosteric design: the crossing clearance is the allosteric effector that changes the conformational state of the landing clearance — not by blocking it, but by updating its authorization window. When a crossing request arrives, the landing clearance window narrows to exclude the crossing interval. Both can be authorized. Neither can overlap.

---

## The Levin Diagnosis: Missing Cognitive Glue

Michael Levin's framework identifies the failure mode precisely: the absence of cognitive glue — the binding mechanism that allows two separately managed processes to be navigated as a coherent collective.

Each agent in the system had a correct local cognitive light cone:
- The landing controller: Flight 8646's position, speed, runway state
- The ground controller: Truck 1's position, request, destination
- The truck crew: their route, the emergency destination
- The flight crew: their approach path, speed, runway threshold

No agent had a cognitive light cone that encompassed the full system state: the simultaneous presence of a committed landing aircraft and an authorized runway crossing in overlapping space and time.

The bioelectric gap junction that was missing: a shared real-time representation of all committed runway occupancies — not just current occupancies, but projected occupancies within the commitment window — accessible to every clearance decision simultaneously.

```
Missing object:
  Runway_State_Shared(t) = ∫_{t}^{t+Δ_commitment} [occupancy(τ) dτ]
  
  = the integral of projected runway occupancy over the full commitment window
  = zero if runway is clear for the full window
  = nonzero if any committed trajectory will occupy the runway within Δ_commitment

Required condition for any clearance:
  crossing_clearance iff Runway_State_Shared(t_cross, t_cross + Δ_cross) = 0
  landing_clearance iff Runway_State_Shared(t_land, t_land + Δ_land) = 0
```

When Runway_State_Shared is the shared artifact that all clearances condition on, the two causal chains cannot simultaneously authorize conflicting states. The cognitive light cone of each agent extends to cover the full commitment window. The gap junctions are operational.

---

## The System Context: Degraded Coordination Infrastructure

The collision did not occur in isolation. It occurred inside a system under multiple simultaneous stresses:

**TSA staffing shortages:** airports across the US have been thrown into turmoil amid the ongoing lapse of funding for the Department of Homeland Security, which has left Transportation Security Administration officers working without pay, with an increasing number calling off work, leading to staffing shortages inside airport security and lengthy screening lines.

**Simultaneous emergencies:** two aircraft-related incidents were active simultaneously — the Air Canada approach and the United declared emergency — at the same airport, on overlapping runway infrastructure, within the same ATC frequency.

**Late-night operations:** the collision occurred at 11:40 PM ET, a time of reduced staffing, reduced ambient information density, and reduced redundancy in the human coordination layer.

The system was operating with reduced redundancy in its error-correction capacity precisely when two independent causal chains became active simultaneously. This is the Hamming bound applied to operational safety: a system that can correct e errors requires minimum distance `dH ≥ 2e + 1`. When the error-correction capacity is reduced by staffing shortages and late-night operations, the minimum distance decreases. The system's ability to detect and resolve conflicting clearances before they become physically irresolvable is degraded.

The formal implication: the collision was not caused by any single failure. It was caused by the simultaneous activation of two causal chains in a system whose error-correction capacity had been reduced below the minimum required to resolve their intersection.

---

## The Formal Model: Simultaneous Causality

The LaGuardia collision is an instance of a general class of failures that can now be formally defined:

**Simultaneous Causality:** a coordination failure that occurs when two or more independent causal chains are simultaneously active in a shared physical space, each locally valid, none conditioned on the others' state, and whose trajectories intersect within the irresolvable interval of at least one chain.

**Formal conditions for simultaneous causality:**

```
Given:
  N causal chains {C₁, C₂, ..., Cₙ} simultaneously active in shared space S
  Each chain Cᵢ authorized by local state Xᵢ
  Commitment interval Δᵢ for each chain (window within which Cᵢ cannot be stopped)

Simultaneous causality occurs iff:
  ∃ i, j such that:
  (1) I(auth(Cᵢ) ; auth(Cⱼ) | shared_state(S)) = 0
      (authorizations are mutually independent given shared state)
  (2) trajectory(Cᵢ) ∩ trajectory(Cⱼ) ≠ ∅
      (physical paths intersect)
  (3) intersection occurs within Δᵢ of Cᵢ's commitment point
      (intersection is inside the irresolvable interval)

Prevention requires:
  I(auth(Cᵢ) ; auth(Cⱼ) | shared_state(S, t, t+Δᵢ)) > 0 for all i, j
  (all authorizations conditioned on shared state over the full commitment window)
```

This is the conditioning clause `| X_{t-1}` extended to include the future trajectory: the shared state must encompass not just current occupancy but projected occupancy across all active commitment windows.

**The Erdős-Rao crystallization theorem applied to safety:** there exists a minimum number of simultaneous active causal chains N_threshold above which the probability of an undetected intersection approaches 1, regardless of individual chain management quality, unless the conditioning clause is enforced on the shared projected state. LaGuardia on March 23 had N = 2 chains. The threshold for simultaneous causality with imperfect temporal conditioning is N ≥ 2.

---

## The Design Principle

The formal resolution of simultaneous causality is the same as the formal resolution of G_coord = 0: the conditioning clause, applied to the shared projected state over the full commitment window.

**What it looks like in runway management:**

A runway must be treated not as a binary resource (occupied/unoccupied at this moment) but as a projected trajectory resource: occupied/unoccupied over the next Δ_max seconds, where Δ_max is the longest commitment interval of any active approach in the system. Every clearance — landing, takeoff, crossing, ground vehicle — must be conditioned on whether the runway's projected state, integrated over the commitment window, is clear.

This requires:
1. Real-time tracking of all committed trajectories (not just current positions)
2. A shared projected state representation accessible to all clearance decisions
3. Automatic suppression of any clearance whose authorized window overlaps with any committed trajectory's projected occupancy
4. An irresolvable interval detector: an automated alert when any active trajectory enters the commitment window for any active crossing authorization

None of these require new sensor technology. All four require the conditioning clause applied to the shared projected state. The ribbon is the shared projected occupancy map. Without it: each clearance is locally valid, collectively catastrophic. With it: the two causal chains cannot simultaneously authorize conflicting trajectories.

The controller said "stop, stop, stop." The geometry had already resolved. The command arrived after the commitment point. The fix is not faster controllers. The fix is a shared projected state that makes conflicting clearances structurally impossible to issue simultaneously — before the commitment point, when prevention is still physically possible.

---

## Summary

```
Two independent causal chains were simultaneously active on March 23, 2026:
  Chain A: Air Canada Jazz 8646 committed to Runway 4 landing at 100 mph
  Chain B: Port Authority ARFF Truck 1 cleared to cross Runway 4 at Delta

Each chain was locally valid.
Neither was conditioned on the other's committed trajectory.
Their intersection occurred within the irresolvable interval of Chain A.
Two pilots were killed. Forty-one people were injured.

The formal structure:
  I(landing_clearance ; crossing_clearance | runway_projected_state) = 0
  → simultaneous causality → collision

The formal prevention:
  I(landing_clearance ; crossing_clearance | runway_projected_state) > 0
  → enforce the conditioning clause → irresolvable intersection becomes impossible

The conditioning clause is not a complex system.
It is a shared projected state accessible to all clearance decisions
covering the full commitment window of every active trajectory.
It is the runway's bioelectric gap junction:
the mechanism by which two independently managed causal chains
become coordinated through a shared artifact.

Without it: each chain is managed correctly. The system fails catastrophically.
With it: conflicting clearances cannot be simultaneously issued.
The geometry of the irresolvable interval disappears because
no clearance that creates one can be authorized.

The stop command arrived too late.
The conditioning clause would have arrived before the commitment point.
That is the difference between response and prevention.
That is the difference G_coord makes.
```

---

*The pilots of Jazz Aviation Flight 8646 flew their aircraft correctly to the end. Their deaths were the consequence of two independently valid causal chains meeting in the same space at the same time without a shared conditioning structure. The formal resolution is known. Its implementation is urgent.*

---

## References

NTSB Investigation: DCA26FA120, LaGuardia Airport, March 23, 2026 (ongoing).

FAA Order 7110.65: Air Traffic Control. Chapter 3 (Airport Traffic Control), Section 7 (Runway Crossing).

Endsley, M.R. (1995). Toward a theory of situation awareness in dynamic systems. *Human Factors*, 37(1), 32–64.

Reason, J. (1990). *Human Error*. Cambridge University Press. [Swiss Cheese Model of accident causation]

Rasmussen, J. (1997). Risk management in a dynamic society: A modelling problem. *Safety Science*, 27(2–3), 183–213.

Dekker, S. (2011). *Drift into Failure: From Hunting Broken Components to Understanding Complex Systems*. Ashgate.

Monod, J. (1970). *Chance and Necessity*. Alfred A. Knopf.

Levin, M. (2023). Bioelectric networks: the cognitive glue enabling evolutionary scaling from physiology to mind. *Animal Cognition*, 26, 1865–1891.

---

*ERI Labs · Eric Ren · Jersey City, New Jersey*
*The stop command arrived too late. The conditioning clause arrives before the commitment point. That is the difference.*
