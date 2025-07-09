# Video Survey Platform

A web-based video survey application that mimics short-form video platforms like TikTok for research purposes. Users can scroll through videos and ads, like content, and interact with the interface while their engagement data is collected for analysis.

## Features

- **TikTok-like Interface**: Vertical scrolling video feed with mobile-optimized design
- **Mixed Content**: Alternating pattern of 2 videos → 1 ad → repeat
- **User Interactions**: Like/unlike videos and ads, play/pause, audio toggle
- **Time Management**: 12-minute session limit with 2-minute warning
- **Data Collection**: Tracks user likes and engagement patterns
- **Admin Dashboard**: Real-time statistics and data management
- **Research-Focused**: Designed for academic/research studies

## System Requirements

### Required Software
- **Python 3.7+** with pip
- **Modern web browser** (Chrome, Firefox, Safari, Edge)
- **Operating System**: Windows, macOS, or Linux

### Python Dependencies
The following packages will be installed automatically:
- `Flask` - Web framework
- `sqlite3` - Database (included with Python)
- `uuid` - User ID generation (included with Python)
- `os` - File system operations (included with Python)
- `random` - Content shuffling (included with Python)
- `datetime` - Timestamp handling (included with Python)

## Installation

1. **Clone or download** this repository to your computer

2. **Navigate to the project directory**:
   ```bash
   cd video-survey-platform
   ```

3. **Install Flask** (if not already installed):
   ```bash
   pip install Flask
   ```

4. **Add your video content**:
   - Place regular videos in the `static/videos/` folder
   - Place advertisement videos in the `static/ads/` folder
   - Supported format: MP4 files
   - Current setup includes 10 ads (vid1.mp4 - vid10.mp4) and 1 test video

## Usage

### Starting the Application

1. **Run the Flask application**:
   ```bash
   python app.py
   ```

2. **Access the platform**:
   - Main survey interface: http://localhost:8000
   - Admin dashboard: http://localhost:8000/admin

### For Participants

1. Open the main interface in a web browser
2. Read the welcome instructions
3. Click "I understand" to begin the session
4. Scroll through videos using:
   - Mouse wheel or touchpad
   - Arrow keys (↑/↓)
   - Touch gestures on mobile
5. Interact with content:
   - Click video to play/pause
   - Click heart button to like/unlike
   - Click speaker button to toggle audio
6. Session automatically ends after 12 minutes
7. Complete post-study questions via provided link

### For Researchers

1. Access the admin dashboard at `/admin`
2. Monitor real-time statistics:
   - Total likes on advertisements
   - Unique users who engaged with ads
   - Individual ad performance metrics
3. Use admin controls:
   - **Refresh Stats**: Update dashboard data
   - **Reset All Stats**: Clear all collected data
   - **Refresh Video List**: Reload videos from file system

## File Structure

```
video-survey-platform/
├── static/
│   ├── ads/                    # Advertisement videos
│   │   ├── vid1.mp4
│   │   ├── vid2.mp4
│   │   └── ... (vid3-vid10.mp4)
│   ├── videos/                 # Regular content videos
│   │   └── testvid1.mp4
│   └── index.html              # Main interface
├── app.py                      # Flask backend application
├── likes.db                    # SQLite database (auto-created)
├── README.md                   # This file
└── requirements.txt            # Python dependencies (optional)
```

## Database Schema

The application uses SQLite with two main tables:

### `records` table
- `id`: Primary key
- `filename`: Video file path
- `is_ad`: Boolean flag (0=video, 1=ad)
- `total_likes`: Like count

### `user_likes` table
- `id`: Primary key
- `user_id`: Unique user identifier
- `video_id`: Foreign key to records table
- `timestamp`: When the like occurred

## Configuration

### Video Content Setup
- **Regular Videos**: Place in `static/videos/` directory
- **Advertisements**: Place in `static/ads/` directory
- **File Format**: MP4 recommended for broad compatibility
- **Naming**: Use descriptive filenames (ads can start with "ad" prefix)

### Session Settings
- **Duration**: 12 minutes maximum
- **Warning**: 2-minute warning before timeout
- **Pattern**: 2 videos → 1 ad → repeat
- **Shuffling**: Content order randomized each session

### Post-Study Integration
- Default Qualtrics survey link included
- Modify the survey URL in both `index.html` and `app.py`
- Survey opens in new tab/window

## Customization

### Modifying Session Duration
In `index.html`, change:
```javascript
timeRemaining: 12 * 60,  // 12 minutes in seconds
```

### Changing Video-to-Ad Ratio
In `app.py`, modify the `create_video_sequence()` function:
```python
# Current: 2 videos, 1 ad
# Change numbers in the while loop condition
videos_added < 2  # Modify this number
```

### Survey Link Updates
Update the Qualtrics URL in both files:
- `index.html`: Look for the survey link in popup HTML
- `app.py`: Update the end screen survey link

## Troubleshooting

### Common Issues

1. **Videos won't load**:
   - Check file paths in `static/videos/` and `static/ads/`
   - Ensure files are MP4 format
   - Verify file permissions

2. **Database errors**:
   - Delete `likes.db` file to reset database
   - Restart the application

3. **Port conflicts**:
   - Change port in `app.py`: `app.run(port=8001)`
   - Access via new port: `http://localhost:8001`

4. **Browser compatibility**:
   - Use modern browsers (Chrome, Firefox, Safari, Edge)
   - Enable JavaScript
   - Allow media autoplay

### Performance Tips
- Use compressed MP4 files for faster loading
- Limit individual video file sizes to under 50MB
- Test with your target device/browser combination

## Data Export

The SQLite database can be queried for research analysis:

```python
import sqlite3
conn = sqlite3.connect('likes.db')
cursor = conn.cursor()

# Export all likes data
cursor.execute('''
    SELECT user_id, r.filename, r.is_ad, ul.timestamp
    FROM user_likes ul
    JOIN records r ON ul.video_id = r.id
''')
data = cursor.fetchall()
```

## Security Considerations

- **Local Use Only**: This application is designed for controlled research environments
- **No Authentication**: Users are identified by session-based UUIDs
- **Data Privacy**: All data stored locally in SQLite database
- **Network Access**: Runs on localhost by default

## License

This project is designed for academic research purposes. Please ensure compliance with your institution's research ethics guidelines and obtain necessary approvals before conducting studies.

## Support

For technical issues:
1. Check the troubleshooting section above
2. Verify all system requirements are met
3. Review console output for error messages
4. Test with minimal video content first

## Version History

- **v1.0**: Initial release with core functionality
- Supports video/ad mixing, user interactions, and data collection
- Includes admin dashboard and session management
