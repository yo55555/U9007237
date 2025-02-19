from flask import Flask, request
import requests

app = Flask(__name__)
TOKEN = '8050661041:AAFXOhIjh8ICQdI72Ilo-HtZxAiXctCs_ho'

@app.route('/webhook', methods=['POST'])
def webhook():
    data = request.json
    if 'inline_query' in data:
        handle_inline_query(data['inline_query'])
    return '', 200

def handle_inline_query(inline_query):
    query_id = inline_query['id']
    query_text = inline_query['query']
    results = search_gif(query_text)
    send_inline_response(query_id, results)

def search_gif(query):
    # Implement your GIF search logic here
    return []

def send_inline_response(query_id, results):
    url = f'https://api.telegram.org/bot{TOKEN}/answerInlineQuery'
    payload = {
        'inline_query_id': query_id,
        'results': results
    }
    requests.post(url, json=payload)

if __name__ == '__main__':
    app.run(port=5000)
