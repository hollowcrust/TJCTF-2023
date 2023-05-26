# TJCTF 2023 – Forensics Challenges

## #1: beep-boop-robot

### Description/Source <br />
  <img width="696" alt="Screenshot 2023-05-26 at 17 47 49" src="https://github.com/hollowcrust/TJCTF-2023/assets/72879387/b3fc0d63-2d70-439c-be01-c8d3a298597a"><br /><br />
  
  https://github.com/hollowcrust/TJCTF-2023/assets/72879387/a284d3fe-a4ec-49e2-a64e-6f34a644ac52
  
  Decoding the Morse code signals in the file either by hand or via https://morsecode.world/international/decoder/audio-decoder-adaptive.html gives the following message<br />
    
  ```
  .... .. / .... --- .-- / .- .-. . / -.-- --- ..- / -.. --- .. -. --. .-.-.- / - .... . / ..-. .-.. .- --. / .. ... / - .--- -.-. - ..-. - .... .. ... .. ... .- .-.. .-.. --- -. . .-- --- .-. -..
  ```
  which translates to: `HI HOW ARE YOU DOING. THE FLAG IS TJCTFTHISISALLONEWORD`<br/>
  
### Flag  
  ```
  tjctf{thisisalloneword}
  ```
  
## #2: nothing-to-see <br/>

### Description/Source <br />
  <img width="699" alt="Screenshot 2023-05-26 at 18 09 21" src="https://github.com/hollowcrust/TJCTF-2023/assets/72879387/80277bd5-ab28-43a5-af81-65822775725c"><br/><br/>
  
  Using binwalk, we can extract an underlying password-protected zip file. Checking the EXIF of the original image gives:<br/><br/>
  <img width="532" alt="Screenshot 2023-05-26 at 18 12 39" src="https://github.com/hollowcrust/TJCTF-2023/assets/72879387/2f2a0c0d-29b7-471f-a381-5037e52720a9"><br/><br/>
  The title of the image is indicated as `panda_d02b3ab3`. Use this as the password and unlock the zip file, which gives `flag.txt` file with the flag
  
### Flag <br />
  ```
  tjctf{the_end_is_not_the_end_4c261b91}
  ```
  
