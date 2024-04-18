---
layout: post
title: KAPA bead clean-up protocol for removal of primer dimers from PCR product
date: '2024-04-01'
categories: Protocol
tags: ITS2 Molecular PCR Protocol
---

This post details a protocol for KAPA bead clean up of PCR product to remove primer dimers.  

This protocol was developed for ITS2 amplicon sequencing of larval *Monitpora capitata* samples detailed in [this notebook post](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/ITS2-amplicon-PCR-and-preparation-for-sequencing-20240326/).

# Equipment and Materials

- [KAPA Pure Beads](https://elabdoc-prod.roche.com/eLD/web/pi/en/products/SEQ-KAPA-0161?searchTerm=07983271001&catalog=Researcher&orderBy=Relevance) 
- 80% molecular grade ethanol
- Magnet stand

# Protocol 

This protocol follows the KAPA Biosystems KAPA Pure Beads [protocol for cleanup of fragmented DNA in NGS workflows](https://elabdoc-prod.roche.com/eLD/api/downloads/88253649-670b-ee11-1c91-005056a772fd?countryIsoCode=pi).  

Size selection occurs through manipulation of the KAPA bead to sample volume ratio. This ratio is based on the desired fragment length to be retained.  

| Fragments to be retained | Recommended KAPA Pure Beads to Sample volumetric ratio |
|--------------------------|--------------------------------------------------------|
| >= 1Kb                   | 0.5X                                                   |
| >=450 bp                 | 0.6X                                                   |
| >=350 bp                 | 0.7X                                                   |
| >=300 bp                 | 0.8X                                                   |
| >=250 bp                 | 0.9X                                                   |
| >=150 bp                 | 1.5X                                                   |
| >=100bp                  | 2.2-3X                                                 |

Our samples have primer dimer at ~100bp with target amplicon length at >300bp.  

Therefore, the protocol written below includes steps for a ratio of 1.5x to remove fragments <150 bp.  

## Protocol steps 

1. Ensure that KAPA Pure Beads has been equilibrated
to room temperature and that the beads are fully
resuspended before proceeding.

2. Add 37.5 µL of KAPA Pure Beads to the 25 µL
fragmented DNA sample.

3. Mix thoroughly by vortexing and/or pipetting up
and down multiple times.

4. Incubate the tubes at room temperature for
15 min to bind the DNA to the beads.

5. Place the tubes on a magnet to capture the
beads. Incubate until the liquid is clear.

6. Carefully remove and discard the supernatant.

7. Keeping the tubes on the magnet, add
200 µL of 80% ethanol. 

8. Incubate the tubes on the magnet at room
temperature for ≥30 sec.

9. Carefully remove and discard the ethanol.

10. Keeping the plate/tube(s) on the magnet, add
200 µL of 80% ethanol.

11. Incubate the plate/tube(s) on the magnet at room
temperature for ≥30 sec.

12. Carefully remove and discard the ethanol. Try
to remove all residual ethanol without disturbing
the beads.

13. Dry the beads at room temperature for 3 – 5 min, or until all of the ethanol has evaporated. The beads should have a matte appearance without any cracking (caused by over drying). Caution: over-drying the beads may result in reduced yield.

14. Remove the plate/tube(s) from the magnet

15. Resuspend the beads in 25 uL
of elution buffer (10 mM Tris-HCl, pH 8.0 – 8.5) or
PCR-grade water, depending on the downstream
application. Here, we will use an alkaline elution buffer (10 mM Tris-HCl). *We chose 25uL as the elution volume because it was the original sample input volume. We will test this protocol and revise if necessary.* 
  
16. Incubate the plate/tube(s) at room temperature for 8 min to elute the DNA off the beads. The elution time may be extended up to 10 min if necessary to improve DNA recovery.

17. Place the plate/tube(s) on a magnet to capture the beads. Incubate until the liquid is clear

18. Transfer the clear supernatant to a new plate/
tube(s). Proceed with your downstream
application, or store DNA at -20°C.

19. Run a QC gel to confirm clean up was successful.  




