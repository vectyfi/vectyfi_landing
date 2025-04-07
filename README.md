# VectyFi Landing Page

![VectyFi Logo](assets/logo.png)

Welcome to the **VectyFi Landing Page** repository! This is the official landing page for [vectyfi.com](https://vectyfi.com), the home of VectyFi—an AI-driven crypto trading dashboard designed to help traders and developers make smarter decisions. Whether you're a retail trader looking for real-time data and AI predictions or a developer wanting to integrate our upcoming API into your trading bots, this landing page is your gateway to exploring VectyFi.

## 📖 About VectyFi
VectyFi is a powerful crypto trading dashboard that streams live data from Binance, calculates technical indicators (e.g., RSI, MACD, Bollinger Bands), and provides AI-driven predictions to help you trade smarter. Key features include:

- **Real-Time Market Data**: Stream live price data for assets like BTC, ETH, and more.
- **AI-Driven Predictions**: Get actionable insights with our advanced AI model (e.g., "Expect a decline to 82000 for BTC").
- **Technical Indicators**: Analyze trends with industry-standard indicators.
- **Upcoming API**: Integrate VectyFi’s data and predictions into your trading bots or custom apps (join the waitlist!).

Our mission is to empower crypto traders and developers with cutting-edge tools to navigate the volatile crypto markets. Follow us on [X @vectyfi](https://x.com/vectyfi) for updates!

## 🌐 About the Landing Page
This landing page is designed to introduce VectyFi, highlight its key features, and drive user engagement. It’s a single-page, responsive website built with HTML, CSS, and JavaScript, optimized for both desktop and mobile users.

### Features
- **Hero Section**: A bold introduction to VectyFi with calls-to-action to try the dashboard or join the API waitlist.
- **Features Section**: Highlights VectyFi’s core features with visually appealing cards.
- **API Waitlist Section**: Encourages developers to join the waitlist for our upcoming API, with an incentive (1 month free access for early members).
- **Testimonials Section**: Placeholder testimonials to build trust (to be updated with real user feedback).
- **Responsive Design**: Fully responsive layout, ensuring a seamless experience on all devices.
- **Modern Aesthetics**: Dark theme with neon purple (#8B5CF6) and orange (#F97316) accents, matching the VectyFi brand.
- **Custom Fonts**: Uses **BrunoAceSC** for headlines and the logo, and **Inter** for body text, creating a futuristic yet readable design.

### Screenshots
![Hero Section](assets/screenshots/hero-section.png)  
*Hero Section showcasing VectyFi’s value proposition.*

![API Waitlist Section](assets/screenshots/api-waitlist-section.png)  
*API Waitlist Section with a form to join the waitlist.*

## 🚀 Getting Started
The landing page is self-hosted on a Linux server with Cloudflared tunneling to expose it to the internet via [vectyfi.com](https://vectyfi.com). Follow these steps to set up your own instance of the landing page.

### Prerequisites
- A Linux server (e.g., Ubuntu 20.04 or later).
- Nginx installed for serving the static site.
- Cloudflared installed for tunneling.
- (Optional) Docker for self-hosting Supabase (for redundant email storage).
- Access to the VectyFi dashboard URL (to link the "Try Now" button).
- Access to the Flask API endpoint for the waitlist form (exposed via Cloudflared).

### Installation
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/your-username/vectyfi-landing-page.git
   cd vectyfi-landing-page
2. **Update Configuration**:
Replace the placeholder dashboard URL in index.html:
html

<a href="https://dashboard.vectyfi.com" class="btn btn-primary">Try VectyFi Free</a>

Update https://dashboard.vectyfi.com with the actual URL of your VectyFi dashboard.

Update the Flask API endpoint for the waitlist form in the JavaScript code:
javascript

const API_WAITLIST_URL = "https://yourdomain.com/api/waitlist"; // Replace with your Flask API endpoint

Replace https://yourdomain.com/api/waitlist with your actual Flask API endpoint (exposed via Cloudflared).

Set Up Nginx:
Copy the index.html file and assets folder to /var/www/html/vectyfi:
bash

sudo mkdir -p /var/www/html/vectyfi
sudo cp index.html assets/ /var/www/html/vectyfi -r

Configure Nginx (/etc/nginx/sites-available/vectyfi):
nginx

server {
  listen 80;
  server_name vectyfi.com www.vectyfi.com;
  root /var/www/html/vectyfi;
  index index.html;
  location / {
    try_files $uri $uri/ /index.html;
  }
}

Enable the site and restart Nginx:
bash

sudo ln -s /etc/nginx/sites-available/vectyfi /etc/nginx/sites-enabled/
sudo systemctl restart nginx

Set Up Cloudflared:
Install Cloudflared:
bash

wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared-linux-amd64.deb

Authenticate with Cloudflare:
bash

cloudflared login

Create a tunnel:
bash

cloudflared tunnel create vectyfi

Configure the tunnel (~/.cloudflared/config.yml):
yaml

tunnel: vectyfi
credentials-file: ~/.cloudflared/[tunnel-uuid].json
ingress:
  - hostname: vectyfi.com
    service: http://localhost:80
  - service: http_status:404

Route DNS:
bash

cloudflared tunnel route dns vectyfi vectyfi.com

Run the tunnel:
bash

cloudflared tunnel run vectyfi

Update DNS records at your registrar (e.g., Namecheap) to point vectyfi.com to the Cloudflared tunnel (follow Cloudflared’s DNS setup instructions).

(Optional) Set Up Supabase for Redundant Email Storage:
Install Docker:
bash

sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker

Pull and start Supabase using Docker Compose (use the official Supabase Docker Compose file from their GitHub: https://github.com/supabase/supabase):
bash

docker-compose pull
docker-compose up -d

Access the Supabase dashboard (default: http://localhost:8000) and create a waitlist table:
Columns: email (text), timestamp (timestamp, default: now()).

Update the landing page JavaScript to send emails to Supabase as a backup (in addition to the Flask API):
javascript

const SUPABASE_URL = "http://your-supabase-url";
const SUPABASE_KEY = "your-supabase-key";
await fetch(`${SUPABASE_URL}/rest/v1/waitlist`, {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    "apikey": SUPABASE_KEY,
    "Authorization": `Bearer ${SUPABASE_KEY}`,
  },
  body: JSON.stringify({ email }),
});

Expose Supabase via Cloudflared if needed (similar to the landing page setup).

Test the Landing Page:
Visit vectyfi.com to ensure the page loads correctly.

Test the waitlist form by submitting an email. Verify that the email is sent to your Flask API (and optionally stored in Supabase).

Check responsiveness on mobile devices (320px–1200px widths).

 Development
The landing page is a static site built with:
HTML/CSS/JavaScript: Single index.html file with embedded CSS and JavaScript.

Fonts:
BrunoAceSC (Google Fonts) for the logo and headlines.

Inter (Google Fonts) for body text.

External Libraries:
Font Awesome for icons (e.g., clock, brain, chart, X, Discord, Telegram).

Assets:
assets/logo.png: VectyFi logo (stylized “V” with neon purple-to-orange gradient, “VectyFi” text in BrunoAceSC).

assets/screenshots/: Screenshots of the dashboard and API waitlist section (placeholders if unavailable).

Waitlist Form Integration
The API waitlist form sends emails to a Flask API endpoint via a POST request:
Endpoint: Configured in the JavaScript code (API_WAITLIST_URL).

Request: POST with JSON body { "email": "user@example.com" }.

Response: Displays a modal with a success or error message based on the API response.

Optionally, emails can be stored in a self-hosted Supabase instance for redundancy or analytics.
 Contributing
We welcome contributions to improve the VectyFi landing page! Here’s how you can contribute:
Fork the Repository:
bash

git clone https://github.com/your-username/vectyfi-landing-page.git
cd vectyfi-landing-page

Create a Branch:
bash

git checkout -b feature/your-feature-name

Make Changes:
Edit index.html to add features, fix bugs, or improve the design.

Ensure changes align with the VectyFi brand (dark theme, neon accents, BrunoAceSC/Inter fonts).

Test Your Changes:
Open index.html in a browser to verify the changes.

Test responsiveness and form submission.

Submit a Pull Request:
Push your changes:
bash

git add .
git commit -m "Add your feature or fix"
git push origin feature/your-feature-name

Open a pull request on GitHub with a description of your changes.

Contribution Ideas
Add real user testimonials to the Testimonials section.

Improve the design with animations (e.g., fade-in effects for sections).

Add a blog section to share trading tips and VectyFi updates.

Integrate additional analytics (e.g., track button clicks with Google Analytics).

 License
This project is licensed under the MIT License. See the LICENSE file for details.
 Contact
Have questions or feedback? Reach out to us:
Email: hello@vectyfi.com (mailto:hello@vectyfi.com)

X: @vectyfi


---

### **Explanation of the README**
- **Overview**: The README introduces VectyFi and the landing page, providing context for potential users and contributors. It highlights the project’s value proposition (AI-driven trading insights, upcoming API) and links to the live site (vectyfi.com) and X (@vectyfi).
- **Features**: Lists the key features of the landing page (e.g., Hero section, API waitlist form, responsive design) with a focus on user engagement and branding consistency (BrunoAceSC/Inter fonts, neon accents).
- **Screenshots**: Includes placeholders for screenshots to visually showcase the landing page (you can replace these with actual images once the page is live).
- **Getting Started**: Provides detailed instructions for self-hosting the landing page on a Linux server with Cloudflared, including Nginx setup, Cloudflared tunneling, and optional Supabase integration. It also guides you on updating the dashboard URL and Flask API endpoint.
- **Development**: Explains the technical stack (HTML/CSS/JS, Google Fonts, Font Awesome) and the waitlist form integration with your Flask API (and optional Supabase backup).
- **Contributing**: Encourages community contributions with clear steps for forking, making changes, and submitting pull requests. Includes ideas for contributions (e.g., adding testimonials, animations).
- **License and Contact**: Specifies the MIT License (a common choice for open-source projects) and provides contact information for support.

### **How to Use the README**
1. **Create the Repository**:
   - Create a new GitHub repository (e.g., `vectyfi-landing-page`).
   - Add the generated `index.html` file, `assets` folder, and this `README.md` file to the repository.
   - If you don’t have the `index.html` yet, you can use the `instructions.md` to generate it with an AI tool.

2. **Add Screenshots**:
   - Once the landing page is live, take screenshots of the Hero section and API Waitlist section.
   - Save them in the `assets/screenshots/` folder (e.g., `hero-section.png`, `api-waitlist-section.png`) and update the README with the correct paths.

3. **Add the Logo**:
   - If you have the VectyFi logo (stylized “V” with gradient, “VectyFi” text in BrunoAceSC), save it as `assets/logo.png` and update the README.
   - If not, you can generate a placeholder logo using a tool like Canva or Figma with the specified design (BrunoAceSC font, neon gradient).

4. **Publish the Repository**:
   - Push the repository to GitHub and make it public.
   - Share the repository link on X (@vectyfi), Reddit, and Discord/Telegram communities to attract contributors and users.

5. **Update the License**:
   - Create a `LICENSE` file with the MIT License text (you can find a template on GitHub or use a license generator).
   - Update the README link to point to the `LICENSE` file.

### **Next Steps in `system_progress.md`**
The `system_progress.md` file includes a task for creating the landing page (`Create a Landing Page for API Access`). Once the landing page is generated, deployed, and the README is published, you can mark this task as ✅. The next priorities remain:
- Move the "API Access" button to the "Premium Features" section.
- Add a price chart to the dashboard.
- Store waitlist emails in a database (already handled by your Flask API, with optional Supabase backup).

If you’d like help generating the `index.html` file, setting up the GitHub repository, or promoting the landing page, let me know! What do you think about the README?

