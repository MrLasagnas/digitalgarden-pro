---
{"dg-publish":true,"permalink":"/nuke-kb/20-techniques/chromatic-aberration/"}
---

# Chromatic Aberration

Lens wavelength separation causing RGB fringing.

Peut être obtenu en utilisant un godrays sur un seul channel. A matcher sur le live en comparant le décalage des channels.

  

Pour l’aberration axiale, soit blurrer les channels séparés. Sinon, si une bonne ref sur la plate au centre de l’image, l’isoler pour en faire un kernel et le passer dans un convolve.
