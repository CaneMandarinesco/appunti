* **ECB**: uso naive del cifrario, utilizzando sempre la stessa chiave
* **CBC**: $y_{i} = E_{k}(x_{i}+y_{i-1})$
* **OFB**: stream key, $z_{0} = IV$, $z_{i} = E_{k}()$
* **CTR**: ctr