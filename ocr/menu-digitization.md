## Menu Digitization with OCR

### Challenges of traditional OCR when applying to menu digitization
1. Too many different templates of menus 
2. Orientation of the menu in images 
3. Multiple languages in a menu 
4. Multiple fonts and font sizes in a menu 
5. Character accuracy vs sequence accuracy 
6. Noise or blur in the menu images 
7. Difficulty in finding adequate training data 
8. Lack of tools and services that allow easy custom model building


### Simple work flow
I have test using tesseract ocr. However, the first thing I need to solve is detect text block within a image. This is a cluster problem that can be solved by many other algorithms such as [CRNN-Keras](https://github.com/qjadud1994/CRNN-Keras) and [SSD\_detectors](https://github.com/mvoelk/ssd_detectors). Once we identity the text blocks, we can apply OCR over each text block and extract text of out it.

### More comprehensive flow
Comparing with the above flow, a more comprehensive flow will be pre-processing image first before applying anything. 
The flow looks like this:
1. Image pre-processing
2. OCR
3. Information extraction
4. Review

### Reference
- [Menu Digitization with OCR and Deep Learning](https://nanonets.com/blog/menu-digitization-ocr-deep-learning/)
- [Augmented Reality Solution - Food ordering](https://yeppar.com/)

