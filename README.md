# LummaC2 config extractor - ChaCha20 newer version

This configuration extractor was written as a hobby project based on a small subset of samples, primarily:
- `50e5e5c6d0f84c47ea75b91999f0d4f55d37ba44252f69d8b9d3c6561ee77d7b`
- `b40a03ca902f3ac606d8c7934afeb0b964c4ffd9dc54e168582c357b414df7f9`

While the C2 domains for the embedded configuration (i.e., bytes present in the binary itself) can be easily retrieved from both samples,
the `b40a` sample appears to utilize a different algorithm for decrypting Steam user profile URLs. It was verified, however, that the sample 
really does contact Steam for an additional domain. Sandbox analysis can be found here:

- https://app.any.run/tasks/52c023de-dbf2-4698-8abb-886b91450692
- https://tria.ge/250301-zpqm9askz5/behavioral2

Since this was supposed to be a fun weekend project which turned out much longer, I did not invest the time to analyze the mentioned sample,
but great articles on the custom obfuscating compiler used in both samples was written by Google's TI team:

- https://cloud.google.com/blog/topics/threat-intelligence/lummac2-obfuscation-through-indirect-control-flow

> Please note that the logic for this extractor was partially borrowed from [@RussianPanda95](https://github.com/RussianPanda95/Configuration_extractors/tree/main/LummaC2),
kudos to her and the work she has done for the community.

The original analyzed sample and all relevant stages are in the `LummaStages.7z` archive, password is `infected`.

# Usage

The usage is relatively straightforward:

**Clone the repo**

```bash
$ git clone https://github.com/zpetricusic/LummaExtractor
$ cd LummaExtractor
```

**Install `pycryptodome` (or just run `pipenv install`)**

```bash
$ pip install pycryptodome
# or, recommended
$ pipenv install
```

**If `pipenv` was used, spawn the shell**

```bash
$ pipenv shell
```

**Run the file and enter the filepath to the .exe when prompted**

```bash
$ python ConfigExtractor.py
Enter the path to the Lumma file: STAGE_FINAL.bin
[*] Decrypting C2 domains embedded in the binary
[*] Entry does not appear to be a valid domain - all blocks likely exhausted. Breaking.
    Entry (first 8 bytes): 007bd70a847060eb
[*] Attempting to find Steam profile chunk for additional domains
[+] Steam chunk found! Attempting to decrypt
Encrypted link: b'f65fe459e65b94259527da21d523db2...'
Key: 37360826
[+] Link successfully decrypted! URL: hxxps[://]steamcommunity[.]com/profiles/76561199822375128
[*] Getting C2 domain from URL
[+] C2 domain successfully retrieved! Domain: experimentalideas[.]today

[+] Configuration obtained!
{
  "LummaC2": {
    "build_id": "jMw1IE--SHELLS",
    "c2": [
      "VisiwonaryPath[.]top",
      "shiningrstars[.]help",
      "mercharena[.]biz",
      "generalmills[.]pro",
      "stormlegue[.]com",
      "blast-hubs[.]com",
      "blastikcn[.]com",
      "nestlecompany[.]pro",
      "naturewsounds[.]help"
    ],
    "steam_c2": "experimentalideas[.]today"
  }
}
```

# Credits

As mentioned before, the logic was partially borrowed from [@RussianPanda95](https://github.com/RussianPanda95) ([Twitter/X @RussianPanda9xx](https://x.com/russianpanda9xx)), huge thanks goes to her and her research.

# Blog post

The related blogpost is available on Medium: [‘CAPTCHA me if you can’ — unmasking LummaC2, one curtain at a time](https://kerberpoasting.medium.com/captcha-me-if-you-can-unmasking-lummac2-one-curtain-at-a-time-6a3e1deaaf40)
