---
layout: post
title: OCR speed&#58; DevonThink Pro Office vs. ScanSnap S1300M
category: digital
tags: tech
---

That the integrated OCR engine, that ships with DevonThink Pro Office is only the single-core version is  known - it's due to licensing issues and DevonThink would have to charge a lot more if they made available the multi-core version.

I wanted to know, if the Abby FineReader which ships with the Fujitsu ScanSnap S1300M utilizes the multi-core capability. So I ran some tests.

**Though I dont't think, it uses multiple cores, I found the ScanSnap to be 4x faster.**

For a 2,5 pages document, the 'normal' workflow (use the built-in OCR of DT) takes *a lot more time* compared to a slightly different workflow, where the OCR is done by the ScanSnap software. The actual result for the OCR job where 48 seconds vs. 12 seconds.

The caveat is, that this approach doesn't have the queue feature, that DT has, so that it'll have to finish the OCR process before you can feed it the next document. But since the OCR is 4 times faster, this is a minor one, since it will increase your overall throughput.

If you want to follow this, just check "convert to searchable PDF" in the  in the profile settings in ScanSnap. Then uncheck the same in the DevonThink Pro Office preferences.