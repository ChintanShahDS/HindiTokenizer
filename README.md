# HindiTokenizer
Hindi Tokenizer built from Scratch using Byte pair encoding methodology

## Achievements
- Compression ratio acheived is 8.38x with vocab of 5000
  - Note: Compression of just less than 3x is directly due to 3 bytes per character (considering some single byte english characters as well)
- Outputs can be seen at https://huggingface.co/spaces/Chintan-Shah/HindiTokenizerFromScratch

## HindiTokenizer.ipynb
- File used to create the first tokenizer
- Here a standard method of merging the highest occurance of a pair of bytes was used
- But with smaller dataset and lesser vocab size sometimes all the 3 bytes of a Hindi character was not merged together
- Though with larger vocab around 5000 this worked also well
- Data cleaning techniques used
  - Removed new line characters
  - Replace multi space characters with a single space
  - Removed duplicate sentences
- *Old.pkl were created from this version
- Tokenizer building information
  - Inputs
    - Dataset used: Someman/hindi-summarization
    - Set: train
    - Rows: First 5000
    - Char length: 13910763
    - Num sentences: 72863
    - After duplicate removal sentences: 27143
    - Vocabulary size: 5000
  - Outputs
    - Tokens length: 22419678
    - Ids after Byte pair encoding length: 2675439
    - Compression ratio: 8.38X
    - Sample tokens
      - ['हमने', ' उन्हें', ' विशेष', ' क्षेत्रों', ' के', ' बारे', ' में', ' बता', ' दिया', ' है', '।', ' हमने', ' उनसे', ' कदम', ' उठाने', ' (', 'इस', ' दिशा', ' में', ')', ' की', ' अपील', ' की', ' है', '।']
      - For sentence: 'हमने उन्हें विशेष क्षेत्रों के बारे में बता दिया है। हमने उनसे कदम उठाने (इस दिशा में) की अपील की है।'

## HindiTokenizerTrials.ipynb - Tried few out of the box things based on language here
- Created with a more methodical approach and with an understanding of the utf 3 byte structure of Hindi characters
- First merged the first 2 bytes specific to Devanagri script i.e. (224, 164) and (224, 165)
- Then merged the 3rd byte to create single token for each character i.e. ((224, 164), 128-191) and ((224, 165), 128-191)
- This was then run through the iterations to combine frequently occuring characters together
- Data cleaning techniques used
  - Removed new line characters
  - Replace multi space characters with a single space
  - Removed duplicate sentences
- merges.pkl and vocab.pkl was created from this version
- Tokenizer building information
  - Inputs
    - Dataset used: Someman/hindi-summarization
    - Set: train
    - Rows: First 10000
    - Char length: 29053605
    - Num sentences: 145194
    - After duplicate removal sentences: 55487
    - Vocabulary size: 10000
  - Outputs
    - Tokens length: 22419678
    - Ids after Byte pair encoding length: 2675439
    - Compression ratio: 8.38X
    - From 256 to 385 tokens were used for the hindi devnagri script characters
    - Sample tokens
      - ['हमने', ' उन्हें', ' विशेष', ' क्षेत्रों', ' के', ' बारे', ' में', ' बता', ' दिया', ' है', '।', ' हमने', ' उनसे', ' कदम', ' उठाने', ' (', 'इस', ' दिशा', ' में', ')', ' की', ' अपील', ' की', ' है', '।']
      - For sentence: 'हमने उन्हें विशेष क्षेत्रों के बारे में बता दिया है। हमने उनसे कदम उठाने (इस दिशा में) की अपील की है।'

## Observations
- With larger dataset and higher vocabulary the words formed are good representation of language
- With languages having more bytes a proper strategy to combine the bytes first into initial set would be better
- Domain expertise can help forming specific structures and understanding patterns to write the tokenizer logic
- Dataset chosen should have almost all the variations along with a number of them for the tokenizer to work better
- The one pair combination logic is very slow and appropriate techniques can be thought and employed for the same
- Need to figure out ways to probably use GPUs or some parallel processing also to do this work
- Byte pair encoding is a good way to compress the tokens


## Things that can be tried
- Further combining various things like vowels with consonants (e.g. 'ि, 'ों, etc. with क, ख, etc.)
- Trying with different vocab size as well as dataset size combinations
- Checking if we can train a tokenizer itself
