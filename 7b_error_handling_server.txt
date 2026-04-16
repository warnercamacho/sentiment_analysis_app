from flask import Flask, render_template, request
from EmotionDetection import emotion_detector

# ✅ Create Flask app FIRST
app = Flask(__name__)


@app.route("/")
def home():
    return render_template("index.html")


@app.route("/emotionDetector")
def emotion_detector_route():

    # Get input from URL
    text_to_analyze = request.args.get("textToAnalyze")

    # ✅ Handle blank input BEFORE calling API
    if text_to_analyze is None or text_to_analyze.strip() == "":
        return "Invalid text! Please try again!"

    # Call emotion detection function
    response = emotion_detector(text_to_analyze)

    # Extra safety check (API failure case)
    if response is None or response.get("dominant_emotion") is None:
        return "Invalid text! Please try again!"

    # Format response
    return (
        f"For the given statement, the system response is "
        f"'anger': {response['anger']}, "
        f"'disgust': {response['disgust']}, "
        f"'fear': {response['fear']}, "
        f"'joy': {response['joy']} and "
        f"'sadness': {response['sadness']}. "
        f"The dominant emotion is {response['dominant_emotion']}."
    )


# ✅ CRITICAL: run block MUST be at bottom and must execute
if __name__ == "__main__":
    print("Starting Flask server on port 5000...")
    app.run(host="0.0.0.0", port=5000, debug=True)