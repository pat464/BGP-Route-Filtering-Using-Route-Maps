BGP Route Filtering Using Route Maps
====================================
Route maps have the capability to filter networks much the same way as ACLs. However, they also provide 
additional capability through the addition or modification of network attributes. In BGP, route 
maps are critical in modifying a unique rout
ing policy on a neighbor-by-neighbor basis
A route map has four components:
-Sequence number: Dictates the processing order of the route map.
-Conditional matching criteria: Identifies prefix characteristics (the prefix itself, BGP 
path attribute, next hop, and so on) for a specific sequence.
-Processing action: Permits or denies the prefix.
-Optional action: Allows for manipulations, depending on how the route map is referenced on the router. Actions can include modification, addition, or removal of route 
characteristics.
======================
A route map uses the command syntax:
#route-map route-map-name [permit | deny] [sequence-number].

#The following rules apply to route map statements:
- If a processing action is not provided, the default value permit is used.
- If a sequence number is not provided, the sequence number is incremented by 10 
automatically.
- If a matching statement is not included, an implied match all prefixes is associated 
with the statement.
- Processing within a route map stops after all optional actions have processed
- An implicit deny or drop is associated for prefixes that are not associated with a 
permit action

========================
For this lab, I will use route-maps to modify BGP attributes at two different policy points (inbound and outbound) in the same topology.
************************

Use route-maps on R2 (the transit router) to:
Inbound from R1 — set differentiated LOCAL_PREF per prefix based on business priority
Outbound to R3 — AS-path prepend one specific prefix, making it look less attractive to R3 without denying it

#High Level Architecture Diagram

   AS 65100                    AS 65200                    AS 65300
  +--------+                  +--------+                  +--------+
  |   R1   |------------------|   R2   |------------------|   R3   |
  +--------+   10.12.1.0/30   +--------+   10.23.1.0/30   +--------+

#IP Addressing Plan
Link	Subnet
R1–R2	10.12.1.0/30 (R1: .1, R2: .2)
R2–R3	10.23.1.0/30 (R2: .1, R3: .2)

R1 Loopback	Prefix	      Business role (for this lab)
L1	172.16.1.0/24	      Priority network — should look preferred
L2	172.16.2.0/24	      Lower priority — should look less preferred