# L / R / M

L/R/M là thứ tự box in-scope theo XML của từng frame.

## adasind_060000.jpg
- L1+R1: LR_noM (center)
- L2+R3: LR_noM (center)
- L3+R4: LR_noM (edge)
- L4+R6+M8: LRM (mid)
- L6+R2: LR_noM (mid)
- L5+R10+M5: LRM (mid)
- L9+R7+M6: LRM (center)
- L8+R8: LR_noM (center)
- L7+R5+M4: LRM (mid)
- R9: R_only (center)
- M2: M_only (mid)
- M3: M_only (mid)
- M7: M_only (mid)
- M9: M_only (center)
- M10: M_only (center)
- M11: M_only (center)
- M12: M_only (center)
## adasind_086220.jpg
- L1+R1: LR_noM (mid)
- L3+R2+M1: LRM (center)
- L5+R5: LR_noM (mid)
- L4+R3+M4: LRM (center)
- L2: L_only (mid)
- L6+M8: LM_noR (center)
- R4: R_only (center)
- M2: M_only (center)
- M3: M_only (center)
- M5: M_only (mid)
- M6: M_only (mid)
- M7: M_only (mid)
## adasind_102750.jpg
- L2+R2+M2: LRM (edge)
- L3+R1+M1: LRM (mid)
- L4+R3: LR_noM (mid)
- L1: L_only (center)
- R4: R_only (edge)
- R5+M8: RM_noL (center)
- M3: M_only (mid)
- M4: M_only (mid)
- M5: M_only (mid)
- M6: M_only (center)
- M7: M_only (mid)

## Zone × cell
| zone | LRM | LR_noM | LM_noR | L_only | RM_noL | R_only | M_only |
|---|---|---|---|---|---|---|---|
| center | 3 | 3 | 1 | 1 | 1 | 2 | 7 |
| mid | 4 | 4 | 0 | 1 | 0 | 0 | 10 |
| edge | 1 | 1 | 0 | 0 | 0 | 1 | 0 |
