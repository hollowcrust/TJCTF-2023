# TJCTF 2023 – Forensics Challenges

## #1: beep-boop-robot

### Description/Sources<br />

<img width="698" alt="Screenshot 2023-05-26 at 22 09 18" src="https://github.com/hollowcrust/TJCTF-2023/assets/72879387/442789f0-63b7-4294-9d3c-5a638f0a4b7f"><br />
  
  https://github.com/hollowcrust/TJCTF-2023/assets/72879387/a284d3fe-a4ec-49e2-a64e-6f34a644ac52
  
  Decoding the Morse code signals in the file either by hand or via https://morsecode.world/international/decoder/audio-decoder-adaptive.html gives the following message<br />
    
  ```
  .... .. / .... --- .-- / .- .-. . / -.-- --- ..- / -.. --- .. -. --. .-.-.- / - .... . / ..-. .-.. .- --. / .. ... / - .--- -.-. - ..-. - .... .. ... .. ... .- .-.. .-.. --- -. . .-- --- .-. -.. .-.. -- .- ---
  ```
  which translates to: `HI HOW ARE YOU DOING. THE FLAG IS TJCTFTHISISALLONEWORDLMAO`<br/>
  
### Flag  
  ```
  tjctf{thisisallonewordlmao}
  ```
  
## #2: nothing-to-see <br/>

### Description/Sources<br />

<img width="696" alt="Screenshot 2023-05-26 at 22 10 12" src="https://github.com/hollowcrust/TJCTF-2023/assets/72879387/42f45a61-4362-4ef9-89b1-d210faf9a7a1"><br/>

### nothing.png
![nothing](https://github.com/hollowcrust/TJCTF-2023/assets/72879387/a8535a72-53e0-4104-b394-f4e52ed14946)<br/>
  
  Using `binwalk` on the image, we can extract an underlying password-protected zip file. Checking the EXIF of the original image using `exiftool` gives:<br/><br/>
  <img width="532" alt="Screenshot 2023-05-26 at 18 12 39" src="https://github.com/hollowcrust/TJCTF-2023/assets/72879387/2f2a0c0d-29b7-471f-a381-5037e52720a9"><br/><br/>
  The title of the image is indicated as `panda_d02b3ab3`. Use this as the password and unlock the zip file, which gives `flag.txt` file with the flag.
  
### Flag <br />
  ```
  tjctf{the_end_is_not_the_end_4c261b91}
  ```
  
