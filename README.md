# imagerecognition

from ibm_watson import VisualRecognitionV4
from ibm_watson.visual_recognition_v4 import FileWithMetadata, RecognizeOptions

# Set up your IBM Cloud Visual Recognition service
apikey = 'IBM_API_KEY'
url = 'API_URL'
version = '2021-09-07'  # You can use the latest version

visual_recognition = VisualRecognitionV4(
    version=version,
    authenticator='YOUR_AUTHENTICATOR',
)
visual_recognition.set_service_url(url)
visual_recognition.set_api_key(apikey)

# Provide an image for classification
image_file = 'image.jpg'

# Classify the image
with open(image_file, 'rb') as image_data:
    classes = visual_recognition.recognize(
        image_file=FileWithMetadata(image_data),
        threshold='0.6',  # You can adjust the threshold based on your needs
    ).get_result()

# Extract and display the results
for class_result in classes['images'][0]['classifiers'][0]['classes']:
    class_name = class_result['class']
    score = class_result['score']
    print(f"Class: {class_name}, Score: {score}")

# To get a description, you can use the 'describe' method
description = visual_recognition.describe(image_file=image_file).get_result()
print(f"Description: {description['images'][0]['description']['captions'][0]['text']}")
