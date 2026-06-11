# Screenshot shot list

Drop PNGs into `assets/` with these exact filenames and they appear automatically.
~1280-1440px, light mode. Terminal captures fine for helm/kubectl steps. No real hostnames/credentials.

| # | File | Module | What to capture |
|---|---|---|---|
| 1 | `assets/vpc-m1-architecture.png` | M1 | the reference architecture diagram or prerequisites checklist view |
| 2 | `assets/vpc-m2-helm.png` | M2 | a successful helm install / pods running (kubectl get pods) |
| 3 | `assets/vpc-m3-inference.png` | M3 | the inference configuration (model endpoint settings) |
| 4 | `assets/vpc-m4-validate.png` | M4 | the validation checks passing (first chat answering in the deployed env) |
| 5 | `assets/vpc-m6-doctor.png` | M6 | the doctor/diagnostic tooling output |

Adding more: copy a `<figure class="shot">` block in `index.html` and set `data-todo`.
