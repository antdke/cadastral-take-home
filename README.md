# OCR Provider Evaluation - Cadastral AI Engineering Take-Home Assignment

## Overview

This project evaluates different OCR (Optical Character Recognition) providers for accuracy and speed in processing a commercial real estate lease document. The OCR providers implemented are:

- **Tesseract**
- **Google Cloud Vision**

The ground truth text is obtained using the Anthropic API with vision mode. The goal is to determine which OCR provider offers the best balance of accuracy and performance for extracting text from PDF documents.

---

## Evaluation Metrics

To assess the performance of each OCR provider, several evaluation metrics are used. These metrics help quantify the accuracy of the OCR outputs compared to the ground truth text.

### 1. **Character Error Rate (CER)**

- **Definition**: The ratio of the Levenshtein distance (edit distance) between the OCR output and the ground truth text to the length of the ground truth text.
- **Why it's useful**: Measures the number of character-level errors, providing fine-grained insight into OCR accuracy.
- **Calculation**:
  $$
  \text{CER} = \frac{\text{Levenshtein Distance}(\text{Ground Truth}, \text{OCR Output})}{\text{Number of Characters in Ground Truth}}
  $$

### 2. **Word Error Rate (WER)**

- **Definition**: Similar to CER but calculated at the word level.
- **Why it's useful**: Evaluates the accuracy of word recognition, crucial for understanding the content.
- **Calculation**:
  $$
  \text{WER} = \frac{\text{Levenshtein Distance}(\text{Ground Truth Words}, \text{OCR Output Words})}{\text{Number of Words in Ground Truth}}
  $$

### 3. **Fuzzy String Match Score**

- **Definition**: A similarity score based on the Levenshtein distance, normalized to a percentage between 0 and 100.
- **Why it's useful**: Provides an overall similarity measure, combining both character and word-level differences.
- **Calculation**: Computed using the `fuzz.ratio` function from the `fuzzywuzzy` library.

### 4. **BLEU Score**

- **Definition**: Measures the n-gram overlap between the OCR output and the ground truth text.
- **Why it's useful**: Originally used for machine translation evaluation, it indicates how closely the OCR output matches the ground truth in terms of sequences of words.
- **Calculation**: Uses cumulative n-gram precision, typically up to 4-grams, calculated using the `sentence_bleu` function from NLTK.

### 5. **ROUGE Scores**

- **Definition**: Recall-Oriented Understudy for Gisting Evaluation (ROUGE) scores, including ROUGE-1, ROUGE-2, and ROUGE-L, measure the overlap of unigrams, bigrams, and the longest common subsequence between the OCR output and the ground truth.
- **Why it's useful**: Useful for evaluating text summarization and the completeness of the OCR output.
- **Calculation**: Calculated using the `python-rouge` library.

### 6. **Confidence Weighted Accuracy (CWA)**

- **Definition**: An accuracy measure that weights correct recognitions by their confidence scores.
- **Why it's useful**: Accounts for the OCR engine's confidence in its predictions, offering insight into the reliability of the results.
- **Calculation**:

  $$
  \text{CWA} = \frac{\sum_{\text{Correct Words}} \text{Confidence}_i}{\sum_{\text{All Words}} \text{Confidence}_i}
  $$

- **Note**: Requires confidence scores for each word from the OCR engine.

---

## How Metrics Determine Accuracy

These metrics collectively provide a comprehensive evaluation of OCR performance:

- **CER and WER**: Highlight the number of errors at the character and word levels.
- **Fuzzy String Match Score**: Gives an overall similarity percentage, useful for quick comparisons.
- **BLEU and ROUGE Scores**: Assess the quality of the OCR output in terms of sequence matching and content preservation.
- **CWA**: Incorporates the OCR engine's confidence, indicating the trustworthiness of the recognized text.

By analyzing these metrics, we can determine which OCR provider offers the best accuracy for our specific use case.

---

## Considerations for Future Work

### **Scalability**

- **Batch Processing**: Implement batch processing for handling multiple documents efficiently.
- **Distributed Computing**: Utilize distributed systems like Apache Spark for large-scale OCR tasks.
- **Asynchronous Operations**: Use asynchronous I/O to improve performance during network-bound operations.

### **Production Environment**

- **API Rate Limits**: Implement rate limiting and exponential backoff strategies to handle API quotas gracefully.
- **Error Handling and Monitoring**: Enhance logging and integrate with monitoring tools like Prometheus or ELK Stack for real-time insights.
- **Resource Optimization**: Profile the application to optimize CPU and memory usage, ensuring it can handle production loads.

### **Extensibility**

- **Modular Design**: Structure the codebase to easily integrate additional OCR providers.
- **Configuration Management**: Use configuration files (YAML/JSON) for managing settings, making the application adaptable to different environments.
- **Continuous Integration/Deployment**: Set up CI/CD pipelines for automated testing and deployment, ensuring code quality and reliability.

---

## Function Documentation

Below is an overview of the key functions and classes in the project, explaining their purpose and how they contribute to the overall solution.

### **`BaseOCR` Class**

- **Purpose**: An abstract base class defining a standard interface for OCR engines.
- **Why it helps**: Promotes a consistent structure, making it easier to add new OCR providers and maintain the codebase.

### **`TesseractOCR` Class**

- **Purpose**: Implements OCR functionality using Tesseract by inheriting from `BaseOCR`.
- **Why it helps**: Encapsulates Tesseract-specific logic, allowing for easy updates and maintenance.

### **`GoogleCloudVisionOCR` Class**

- **Purpose**: Implements OCR functionality using Google Cloud Vision API by inheriting from `BaseOCR`.
- **Why it helps**: Manages API interactions efficiently, abstracting away complexities.

### **`preprocess_image(image)`**

- **Purpose**: Enhances the image for better OCR accuracy through grayscale conversion, denoising, thresholding, and skew correction.
- **Why it helps**: Improves OCR results by providing cleaner input images, which is crucial for text recognition.

### **`convert_pdf_to_images(pdf_path, dpi=300)`**

- **Purpose**: Converts each page of a PDF into high-resolution images.
- **Why it helps**: Allows the OCR engines, which typically work on images, to process PDF documents.

### **`evaluate_ocr(ground_truth, ocr_result)`**

- **Purpose**: Compares OCR results against the ground truth using various metrics.
- **Why it helps**: Quantifies the accuracy of OCR outputs, facilitating objective comparisons.

### **`evaluate_all_ocrs(ground_truth, ocr_results)`**

- **Purpose**: Evaluates multiple OCR results and compiles the metrics into a DataFrame.
- **Why it helps**: Provides a consolidated view of all OCR providers' performance for analysis.

### **`per_page_analysis(ground_truth, ocr_result)`**

- **Purpose**: Analyzes OCR performance on a per-page basis.
- **Why it helps**: Identifies specific pages where OCR accuracy may be lacking, aiding in targeted improvements.

### **`save_checkpoint(data, filename)` and `load_checkpoint(filename)`**

- **Purpose**: Save and load intermediate results to avoid redundant processing, especially with time-consuming API calls.
- **Why it helps**: Improves efficiency and resource utilization by caching results.

---

## Time Spent

### Total time spent on the assignment: 9 hours

- Understanding requirements: 1 hour
- Implementing OCR providers: 3 hours
- Implementing evaluation metrics: 3 hours
- Testing and debugging: 1.5 hours
- Writing documentation and readme: 0.5 hours

NOTE: I had implementation for Mathpix as well. But, I removed it towards the end because implementation became too cumbersome for the time allotted compared to the other providers.

---

## Usage Instructions

### **Setup**

1. **Clone the Repository**: Clone the project repository to your local machine.

2. **Install Dependencies**: Install the required packages using:

   ```bash
   pip install -r requirements.txt
   ```

3. **Set API Keys and Credentials**:

   - **Anthropic API**:
     - Set `ANTHROPIC_API_KEY` in your environment variables.
   - **Google Cloud Vision API**:
     - Set `GOOGLE_APPLICATION_CREDENTIALS` to the path of your service account JSON key file.

### **Running the Jupyter Notebook**

1. **Run the Jupyter Notebook**:

   ```bash
   jupyter notebook Untitled.ipynb
   ```

- The script will perform OCR using the implemented providers and evaluate the results.

### **Results**

- Evaluation results are printed to the console and saved as checkpoint files for future reference.
- Visualizations are displayed using Matplotlib, showing comparative performance metrics.

## Conclusion

[Results Example](https://docs.google.com/document/d/1QXRE24EvqUBpr-CCHUs-qAr7RmzkkqlWNbtvdqHKJbw/edit?usp=sharing)

🌟 **Tesseract is the recommended OCR provider to its superior accuracy.**

If processing speed is a higher priority than accuracy, and minor inaccuracies are acceptable, you might consider Google Cloud Vision.

However, since you're handling important documents like commercial real estate leases, accuracy is paramount. In this category, Tesseract outperforms.

**Speed Metrics:**

- Google Cloud Vision is faster (average processing time per page of 0.91 seconds compared to Tesseract's 10.33 seconds)

**Accuracy Metrics:**

- **Lower Character Error Rate (CER):** Tesseract has a CER of 0.560, compared to Google Cloud Vision's 0.656, indicating fewer character-level errors.
- **Lower Word Error Rate (WER):** Tesseract's WER is 0.701, whereas Google Cloud Vision's is 0.806, showing better word recognition.
- **Higher Fuzzy Match Score:** Tesseract scores 0.66 versus Google Cloud Vision's 0.60, reflecting a closer overall text similarity to the ground truth.
- **Per-Page Analysis Consistency:** Across individual pages, Tesseract consistently demonstrates lower error rates and higher match scores, confirming its reliability.
- **Confidence Weighted Accuracy (CWA):** Tesseract provides confidence scores, resulting in a measurable CWA of 0.0107. Google Cloud Vision lacks a one-to-one correspondence between words and confidence scores, making it difficult to assess the trustworthiness of its outputs.
