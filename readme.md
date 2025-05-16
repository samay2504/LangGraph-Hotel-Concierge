# Hotel Booking Agent

An AI-powered conversational agent for hotel bookings, built with LangGraph, LangChain, and Google's Gemini AI models. This agent can handle hotel room reservations, booking management, and general inquiries through Instagram DMs.

## 🌟 Features

- **Room Booking**: Process complete booking flows with all necessary details
- **Reservation Management**: Check existing bookings, reschedule dates, and cancel reservations
- **Hotel Information**: Provide details about room types, amenities, prices, and policies
- **Conversational UI**: Natural language interface through Instagram Direct Messages
- **Persistent Storage**: SQLite database for reliable booking data management
- **Context-Aware**: Maintains conversation history for coherent interactions

## 📋 Project Structure

```
hotel-booking-agent/
├── Hotel_Booking_agent.ipynb    # Main Jupyter notebook with all code
├── hotel_bookings.db            # SQLite database for storing bookings
├── apis.env                     # Environment variables (API keys, etc.)
├── README.md                    # This documentation file
└── Langraph Diagrams.png        # Shows the flowchart working of langraph implementation, made using Mermaid.
```

## 🛠️ Technical Architecture

The agent is built on an orchestrated workflow using LangGraph, with several key components:

1. **Intent Classification**: Analyzes user messages to determine their intent (booking, checking, rescheduling, etc.)
2. **Task-Specific Handlers**: Dedicated handlers for each type of user intent
3. **State Management**: Maintains conversation context and booking information
4. **Database Integration**: Persistent storage for booking records
5. **Instagram API Integration**: Webhook-based system for sending/receiving messages

### Workflow Diagram

```
User Message → Intent Classification → Task-Specific Handler → Database Operations → Response Generation → User
```

### State Graph Nodes

- `parse_intent`: Determines user intent from message
- `handle_booking`: Processes room booking requests
- `handle_check_booking`: Retrieves and formats existing bookings
- `handle_reschedule`: Updates booking dates
- `handle_cancel`: Cancels existing bookings
- `handle_room_info`: Provides information about room types
- `handle_general_info`: Answers questions about hotel policies and services
- `handle_greeting`: Responds to initial greetings
- `handle_other`: Manages unclassified requests

## 🏨 Hotel Information

The system is configured with the following hotel details:

### Room Types
- **Standard Room**: $100/night, 2 guests, TV, WiFi, Air Conditioning
- **Deluxe Room**: $150/night, 2 guests, TV, WiFi, AC, Mini Bar, City View
- **Suite**: $250/night, 4 guests, TV, WiFi, AC, Mini Bar, Living Room, Kitchenette, Ocean View

### Hotel Policies
- Check-in: 3:00 PM
- Check-out: 11:00 AM
- Pet policy: Only service animals allowed
- Parking: Valet parking available for $25/day
- WiFi: Complimentary high-speed WiFi
- Breakfast: Continental breakfast included
- Cancellation: Free up to 48 hours before check-in

## 🚀 Setup and Installation

### Prerequisites
- Python 3.11
- Jupyter Notebook or JupyterLab
- Instagram Developer Account
- Google Cloud account (for Gemini API)

### Step 1: Clone the repository
```bash
git clone https://github.com/samay2504/LangGraph-Hotel-Concierge
cd hotel-booking-agent
```

### Step 2: Create and activate a virtual environment
```bash
# Using conda
conda create -n hotel-agent python=3.10
conda activate hotel-agent

# Or using venv
python -m venv hotel-agent
source hotel-agent/bin/activate  # On Windows: hotel-agent\Scripts\activate
```

### Step 3: Install dependencies
```bash
pip install langchain langchain-google python-dotenv flask requests pydantic sqlite3
```

### Step 4: Set up environment variables
Create a `.env` file in the project root with the following variables:
```
GEMINI_API_KEY=your_gemini_api_key
IG_ACCESS_TOKEN=your_instagram_access_token
IG_ACCOUNT_ID=your_instagram_account_id
PAGE_ID=your_facebook_page_id
VERIFY_TOKEN=your_webhook_verification_token
GOOGLE_APPLICATION_CREDENTIALS=path_to_google_credentials_json
```

### Step 5: Configure Instagram Webhook
1. Create a Facebook Developer App
2. Set up a webhook subscription for the Instagram Graph API
3. Configure the webhook URL to point to your deployed agent's `/webhook` endpoint
4. Use the `VERIFY_TOKEN` from your `.env` file for verification

### Step 6: Run the application
```bash
jupyter notebook Hotel_Booking_agent.ipynb
```
Or run the Flask app directly:
```bash
python app.py (yet to create)
```

## 🧪 Testing Locally

The notebook includes a test section with sample conversations that you can use to validate functionality before deployment. The test simulates a conversation with the following flow:

1. Room booking inquiry
2. Providing booking details
3. Checking current bookings
4. Attempting to reschedule
5. Requesting room information
6. Asking about check-in time
7. Cancellation flow

## 📊 Database Schema

The SQLite database contains two tables:

### `bookings` Table
- `id`: Unique booking identifier
- `guest_name`: Customer's name
- `guest_id`: Unique identifier for the guest (Instagram ID)
- `room_type`: Type of room booked (standard, deluxe, suite)
- `check_in`: Arrival date
- `check_out`: Departure date
- `num_guests`: Number of guests
- `booking_time`: When the booking was made
- `status`: Current status (confirmed, cancelled)

### `sessions` Table
- `user_id`: Unique identifier for the user
- `state_json`: Serialized conversation state

## 🔄 Conversation Flow Examples

### Booking a Room
```
User: Hi, I'd like to book a room
Agent: Missing: guest_name, room_type, check_in, check_out, num_guests. Please provide these details.

User: I want a deluxe room from 2025-06-14 to 2025-06-20 for 3 people. My name is Samay Mehar.
Agent: Success! Booking ID: 2
```

### Checking Bookings
```
User: Can you tell me my current bookings?
Agent: Here are your current bookings:
Booking #2: deluxe room from 2025-06-14 to 2025-06-20 for 3 guest(s). Status: confirmed
```

### Room Information
```
User: What types of rooms do you have?
Agent: We offer three room types:
* Standard Room: $100/night, accommodates 2 guests. Amenities: TV, WiFi, Air Conditioning.
* Deluxe Room: $150/night, accommodates 2 guests. Amenities: TV, WiFi, Air Conditioning, Mini Bar, City View.
* Suite: $250/night, accommodates 4 guests. Amenities: TV, WiFi, Air Conditioning, Mini Bar, Living Room, Kitchenette, Ocean View.
```

## 🌐 Deployment

To deploy this agent for production use:

1. Host the Flask application on a cloud provider (AWS, Google Cloud, Heroku, etc.)
2. Ensure your server has HTTPS capability (required for Instagram webhooks)
3. Update your Instagram webhook subscription with the production URL
4. Set up proper error monitoring and logging

## 🔒 Security Considerations

- Store sensitive API keys and tokens in environment variables, never in code
- Implement rate limiting to prevent abuse
- Add validation for all user inputs
- Use HTTPS for all external communications
- Consider encrypting sensitive customer data in the database

## 📝 Future Enhancements

- Payment processing integration
- Room availability checking
- Special requests handling
- Multi-language support
- Rich media responses (images of rooms, amenities)
- Integration with hotel's existing PMS (Property Management System)
- Analytics dashboard for booking statistics

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🙏 Acknowledgments

- LangChain and LangGraph for the agent orchestration framework
- Google for the Gemini LLM model
- Meta for the Instagram Graph API

---

*This project was developed as a demonstration of conversational AI capabilities for the hospitality industry.*
