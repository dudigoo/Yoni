In python Select a code from an externaly defind  dropdown list, each code has a sentence associated to it.

Take the code and explain sentence plus a timestamp in the movie and add the to relevant colums in pdf file. The process repeat for several time stamps in the video

import cv2
import pandas as pd
from reportlab.lib.pagesizes import letter
from reportlab.platypus import SimpleDocTemplate, Table, TableStyle
from reportlab.lib import colors

class VideoCodeAnnotator:
    def __init__(self, video_path, codes_dict):
        """
        Initialize the video code annotator
        
        :param video_path: Path to the video file
        :param codes_dict: Dictionary of codes with their associated sentences
        """
        self.video_path = video_path
        self.codes_dict = codes_dict
        self.annotations = []
        
    def extract_frame_at_time(self, timestamp):
        """
        Extract a frame from video at specific timestamp
        
        :param timestamp: Time in seconds
        :return: Frame image or None
        """
        cap = cv2.VideoCapture(self.video_path)
        
        if not cap.isOpened():
            print(f"Error: Could not open video file {self.video_path}")
            return None
        
        fps = cap.get(cv2.CAP_PROP_FPS)
        frame_number = int(timestamp * fps)
        
        cap.set(cv2.CAP_PROP_POS_FRAMES, frame_number)
        ret, frame = cap.read()
        cap.release()
        
        if not ret:
            print(f"Error: Could not extract frame at {timestamp} seconds")
            return None
        
        return frame
    
    def annotate_video(self, timestamps):
        """
        Annotate video with codes at specific timestamps
        
        :param timestamps: List of timestamps to annotate
        """
        for timestamp in timestamps:
            frame = self.extract_frame_at_time(timestamp)
            
            # Prompt user to select code
            print(f"\nAvailable Codes at {timestamp} seconds:")
            for code, sentence in self.codes_dict.items():
                print(f"{code}: {sentence}")
            
            selected_code = input("Enter code: ")
            
            if selected_code in self.codes_dict:
                self.annotations.append({
                    'Timestamp': timestamp,
                    'Code': selected_code,
                    'Sentence': self.codes_dict[selected_code]
                })
            else:
                print("Invalid code selected.")
    
    def generate_pdf_report(self, output_path):
        """
        Generate PDF report of annotations
        
        :param output_path: Path to save PDF
        """
        # Create DataFrame
        df = pd.DataFrame(self.annotations)
        
        # Create PDF
        doc = SimpleDocTemplate(output_path, pagesize=letter)
        
        # Convert DataFrame to list for PDF table
        data = [df.columns.tolist()] + df.values.tolist()
        
        # Create table
        table = Table(data)
        table.setStyle(TableStyle([
            ('BACKGROUND', (0,0), (-1,0), colors.grey),
            ('TEXTCOLOR', (0,0), (-1,0), colors.whitesmoke),
            ('ALIGN', (0,0), (-1,-1), 'CENTER'),
            ('FONTNAME', (0,0), (-1,0), 'Helvetica-Bold'),
            ('FONTSIZE', (0,0), (-1,0), 12),
            ('BOTTOMPADDING', (0,0), (-1,0), 12),
            ('BACKGROUND', (0,1), (-1,-1), colors.beige),
            ('GRID', (0,0), (-1,-1), 1, colors.black)
        ]))
        
        # Build PDF
        doc.build([table])

# Example usage
if __name__ == "__main__":
    # Define video path
    video_path = "path/to/your/video.mp4"
    
    # Define codes dictionary
    codes = {
        'A1': 'Scene shows character entering room',
        'B2': 'Dialogue between two main characters',
        'C3': 'Action sequence begins',
        # Add more codes as needed
    }
    
    # Timestamps to annotate
    timestamps = [10.5, 25.3, 45.7]
    
    # Create annotator
    annotator = VideoCodeAnnotator(video_path, codes)
    
    # Annotate video
    annotator.annotate_video(timestamps)
    
    # Generate PDF report
    annotator.generate_pdf_report("video_annotations.pdf")
