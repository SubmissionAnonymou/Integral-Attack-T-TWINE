
# Integral Cryptanalysis of Reduced-Round Tweakable TWINE
This is the accompanying code for the article "Integral Cryptanalysis of Reduced-Round Tweakable TWINE". It consists of Four python scripts: `twine.py`, `test_vector.py`, `11r_SingleActiveNibble.py`, `11r_design_paper.py`.

## Test Vector
To validate our implementaion of T-TWINE, we use the test vectors presented in the design paper.
### T-TWINE-80
```py
python test_vector.py -z 80  -p "0123456789abcdef" -k "00112233445566778899" -t "fedcba9876543210"
```
The ciphertext should be:
```
fbb33219433a42f2
```
### T-TWINE-128
```py
python test_vector.py -z 128 -p "0123456789abcdef" -k "00112233445566778899aabbccddeeff" -t "fedcba9876543210"
```
The ciphertext should be:
```
ce9e755fffeca2f8
```
## Check 11-round distinguisher with a single active nibble
In the paper, we presented two 11-round distinguishers with a single active nibble ( nibble 14 or 15). To verify the correctness of the distinguisher, we opt randomly a plaintext `P`, a key `K`, and a constant value of 15 nibble of the tweak `T`. Then, we iterate over all possible value of the active nibble of the tweak and obtain the ciphertext after 11 rounds `C`. After that we calculate the XOR sum of `C` (`Sum(C)`) and find the position of bits with zero-sum. We repeat these steps many times with different `P, K, T` and update the position of bits with zero-sum (`Persistent Balanced Bits`). 

To verfiy the distinguisher where e.g., the nibble 14 in the tweak is active in case of T-TWINE-80, we run:
```py
python 11r_SingleActiveNibble.py -z 80 -n 14
```
The output will be like:
```
_______P________ __________K_________ _______T________ ______________________________Sum(C)____________________________ ____________________Persistent Balanced Bits____________________
f58d099c37e2b91b 43580ddb0422354bd3f8 f50082af0ac260_4 0101010110110100101100001110000010110100111101100001110001100010 0?0?0?0??0??0?00?0??0000???00000?0??0?00????0??0000???000??000?0
736818b06589635d 741a030c76e81705fdb8 962497f014a31c_d 1011111111101011110011101011000010101011100011111101011100000010 ???????????????????????0????0000?0????????????????0?????0??000?0
862307de42a338b4 82621f5f74f32e0efb47 c7b08eae48e241_3 1001011100001010001100001100000011110110010101101001101001101111 ???????????????????????0????0000??????????????????0?????0??0????
18e00324447450b2 40280245fde4db11c099 63f1950781c567_e 0101000001100101001101010011000000111110000101100101100010110101 ????????????????????????????0000??????????????????0?????????????
6159784de75a1ee9 3159d1b6766518ef3c90 475d6317a7e260_3 1011110010110111111110010000000001011010111010001100100000100100 ????????????????????????????0000??????????????????0?????????????
77273d102dff43ef d301f1b82054e37a9187 522cb476610739_3 0111011010001011000000000010000011111111100000001001101110100011 ????????????????????????????0000??????????????????0?????????????
e7e1c3bc99334d82 0797f2b84bd4e55deb8a d83448505ac8e5_9 1111100001000101101101001011000010011101000000111110010111000100 ????????????????????????????0000????????????????????????????????
746ed55d206f92d8 5c7a59f2df8b6fff918f 690e2d729ea39f_7 1000000100101101011011011000000001001001001000100011111110111010 ????????????????????????????0000????????????????????????????????
ac70e9a0d5edb098 16dea13706ce30eded9d 93cc3455d255d1_d 0111010110100110011101001100000011010111000101110110000000001101 ????????????????????????????0000????????????????????????????????
d8aa57180efdc979 e276693b24e53e212cfb e40f0d6f578e3d_1 1010001000111111111101011110000010111001111110110110110001101011 ????????????????????????????0000????????????????????????????????
c98e4a89bb8abc67 63a517c964d4f8814e5c 397054786c5579_3 1010010110100100110111110111000011100101101101100001110100111010 ????????????????????????????0000????????????????????????????????
                               ...                 
```

## Check 11-round distinguisher reported in the design paper
In the design paper, the authors reported an 11-round distinguishers with three active nibbles (5, 10, 11) in the tweak. This distinguisher should lead to two balanced nibbles (0, 11) in the ciphertext side. However, when we test this distinguisher using our MILP model with the same input settings, we confirmed that there is only one balanced nibble (11) in the ciphertext side. To To verify the correctness of the distinguisher, we opt randomly a plaintext `P`, a key `K`, and a constant value of 13 nibble of the tweak `T`. Then, we iterate over all possible value of the active nibbles (5, 10, 11) of the tweak and obtain the ciphertext after 11 rounds `C`. After that we calculate the XOR sum of `C` (`Sum(C)`) and find the position of bits with zero-sum. We repeat these steps many times with different `P, K, T` and update the position of bits with zero-sum (`Persistent Balanced Bits`). You can run experiment for T-TWINE-80 by running:
```py
python 11r_design_paper.py -z 80
```
The output will be like:
```
_______P________ __________K_________ _______T________ ______________________________Sum(C)____________________________ ____________________Persistent Balanced Bits____________________
e519c1916ab75f82 0747ec832f3acf716055 03386_e531__31ed 0110010001101001000101110101000111111010000100000000101111101010 0??00?000??0?00?000?0???0?0?000??????0?0000?00000000?0?????0?0?0
32d9622b24497d38 a8c9e0104c6128423aee 5df26_152d__a1d5 0110011000100110100110111110010111000011111100000101011001111001 0??00??00??0?????00?????????0?0??????0??????00000?0??????????0??
e06fe724a30533a3 32b893fd62ab5c0e571d 90966_efda__4bca 0011111110011000111000111011001101111010101100000100110100001010 0???????????????????????????0????????0??????00000?0??????????0??
7f9644e6a0c2fcc8 c274b0f5d6215f09dae5 88369_0f60__75b8 1100110001010100000011101001100001001001010100001111100011000011 ?????????????????????????????????????0??????0000?????????????0??
8e14703bc300f775 493abaedb4d6fb51c6a2 c042e_f6a9__603d 1001000101001011000001001110101111101000110100001111110010010001 ?????????????????????????????????????0??????0000?????????????0??
26f0dec8f4b3a9d6 1041c4ed7f81e0122d3f 71cc1_1a47__1255 0001101100001100110001011010110110110101001000000100011111001111 ????????????????????????????????????????????0000????????????????
4abd2f75087ec1aa 137d66dca4c3c7446ffe b266c_1a38__f41f 0001000000011100001010010010110010000110011000001101100010000011 ????????????????????????????????????????????0000????????????????
888e5e120c0c9795 78767f3da5e340ea3a8e 4b93e_0ed7__94ac 0100110010100100110000001101100000010010111000001110101101011001 ????????????????????????????????????????????0000????????????????
038453f44b801852 bde8a206135097580112 390a8_ca79__fa37 0001011110000000001100110001010100111110010100001011010101111011 ????????????????????????????????????????????0000????????????????
3c4dc22b2ef0598c 996c9708a78a34177f1f ea23d_e6cf__fd44 0010110111101010011000011101011010001110110100001111001000111010 ????????????????????????????????????????????0000????????????????
b4b9dc694c0eb701 50bb19cde4e49abc8171 7a02b_a5e7__0644 1011101110101000001110000100111101111010110100000100001111000011 ????????????????????????????????????????????0000????????????????
6d7ad7b96b819248 2d3cc9cafcb54b375d25 ce7ff_683e__5a41 1001110011111010000110100000001110011010111000000001111010011100 ????????????????????????????????????????????0000????????????????
c46019b029cfad75 11b57d0c0fb6b2a93720 bf2fc_e1c3__b11f 0000101000110100010011000000100010000011101000000010101001100001 ????????????????????????????????????????????0000????????????????
1156bd263b849008 28a619a22167a7e2412b 737a8_2feb__0447 0011001110101000001100100110100001001001111100000110111001110010 ????????????????????????????????????????????0000????????????????
dc0b78794ba987dc 9f384520ebbace0efe0c 34c17_8cb8__cdae 0000100000100101101000100010101101011101111100001110010000001011 ????????????????????????????????????????????0000????????????????
0f6d373cdb6f8c2a 5c56ab493c8badfea8a5 76a39_bac2__3750 1010111111011111100101011110001000010100011100001010010010011110 ????????????????????????????????????????????0000????????????????
                               ...                 
```

As we can see, the nibble (0) after 11 rounds in not balanced.


