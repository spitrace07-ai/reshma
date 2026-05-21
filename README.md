# reshma
code
# In your project folder
echo "requests==2.31.0" > requirements.txt
git init
git add .
git commit -m "first commit"
git branch -M master
git remote add origin https://github.com/spitrace07-ai/Spi-Trace.git
git push -u origin master
❌ VULN 7: Weak Hashing - MD5 (CWE-327)
@app.route('/hash', methods=['POST'])
def hash_password():
    password = request.json.get('password')
    # MD5 is broken and should not be used for passwords
    hashed = hashlib.md5(password.encode()).hexdigest()
    return jsonify({"hash": hashed})
 
# ❌ VULN 8: SSRF - Server Side Request Forgery (CWE-918)
@app.route('/fetch', method# Spi-Trace - Vulnerable Demo App
# WARNING: Intentional vulnerabilities for Snyk scanning demo

import os
import sqlite3
import subprocess
import pickle
import hashlib
from flask import Flask, request, jsonify
import requests
import yaml

app = Flask(__name__)

# VULN 1: Hardcoded credentials
SECRET_KEY = "supersecret123"
DB_PASSWORD = "admin1234"
API_KEY = "sk-abc123hardcodedapikey9999"

# VULN 2: Hardcoded JWT Secret
JWT_SECRET = "jwt_do_not_share_abcd1234"

def get_db():
    conn = sqlite3.connect("spi_trace.db")
    return conn

# VULN 3: SQL Injection
@app.route('/user', methods=['GET'])
def get_user():
    username = request.args.get('username')
    conn = get_db()
    cursor = conn.cursor()
    query = f"SELECT * FROM users WHERE username = '{username}'"
    cursor.execute(query)
    result = cursor.fetchall()
    return jsonify({"result": str(result)})

# VULN 4: Command Injection
@app.route('/ping', methods=['GET'])
def ping():
    host = request.args.get('host')
    output = subprocess.check_output(f"ping -c 1 {host}", shell=True)
    return jsonify({"output": output.decode()})

# VULN 5: Insecure Deserialization
@app.route('/load', methods=['POST'])
def load_data():
    data = request.data
    obj = pickle.loads(data)
    return jsonify({"loaded": str(obj)})

# VULN 6: Path Traversal
@app.route('/file', methods=['GET'])
def read_file():
    filename = request.args.get('filename')
    with open(f"./files/{filename}", 'r') as f:
        content = f.read()
    return jsonify({"content": content})

# VULN 7: Weak MD5 Hashing
@app.route('/hash', methods=['POST'])
def hash_password():
    password = request.json.get('password')
    hashed = hashlib.md5(password.encode()).hexdigest()
    return jsonify({"hash": hashed})

# VULN 8: SSRF
@app.route('/fetch', methods=['GET'])
def fetch_url():
    url = request.args.get('url')
    response = requests.get(url)
    return jsonify({"body": response.text[:500]})

# VULN 9: Unsafe YAML
@app.route('/parse', methods=['POST'])
def parse_yaml():
    raw = request.data.decode()
    data = yaml.load(raw)
    return jsonify({"parsed": str(data)})

# VULN 10: Debug mode ON
if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0', port=5000)s=['GET'])
def fetch_url():
    url = request.args.get('url')
    # No URL validation — attacker can hit internal services
    response = requests.get(url)
    return jsonify({"body": response.text[:500]})
 
# ❌ VULN 9: YAML Deserialization (CWE-502)
@app.route('/parse', methods=['POST'])
def parse_yaml():
    raw = request.data.decode()
    # yaml.load without Loader is unsafe
    data = yaml.load(raw)
    return jsonify({"parsed": str(data)})



    
 
# ❌ VULN 10: Debug mode ON in production (CWE-94)
if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0', port=5000)
