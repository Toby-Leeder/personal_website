---
layout: blank
title: "Processing Order..."
permalink: /checkout/
---

<style>
  .checkout-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    text-align: center;
  }
  
  /* Loading Animation */
  .loader {
    border: 5px solid #f3f3f3;
    border-top: 5px solid #a8c7fa; /* Matches the blue button */
    border-radius: 50%;
    width: 60px;
    height: 60px;
    animation: spin 1s linear infinite;
    margin-bottom: 25px;
  }
  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }
  .loading-text {
    color: #666;
    font-size: 1.2em;
    font-weight: 500;
  }

  /* The Popup Modal */
  #success-modal {
    display: none; /* Hidden by default */
    position: fixed;
    z-index: 10;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(248, 249, 250, 0.85); /* Soft transparent background */
    backdrop-filter: blur(5px);
  }
  .modal-content {
    background-color: #fff;
    margin: 10% auto;
    padding: 40px;
    border-radius: 20px;
    width: 90%;
    max-width: 450px;
    box-shadow: 0 15px 35px rgba(0,0,0,0.1);
  }
  .modal-title {
    color: #4f378a;
    font-size: 1.8em;
    margin-bottom: 15px;
    margin-top: 0;
  }
  .crypto-box {
    background: #f1f3f4;
    padding: 15px;
    border-radius: 10px;
    margin: 25px 0;
    font-family: monospace;
    color: #333;
    font-size: 0.9em;
    word-break: break-all;
  }
  .back-btn {
    background-color: #eee;
    color: #555;
    padding: 12px 24px;
    border-radius: 20px;
    text-decoration: none;
    font-weight: 600;
    display: inline-block;
    margin-top: 10px;
  }
</style>

<div class="checkout-wrapper" id="loading-screen">
  <div class="loader"></div>
  <div class="loading-text">
    Securing your (BLEEPED)...<br>
    <span style="font-size: 0.8em; color: #999;">Establishing encrypted connection</span>
  </div>
</div>

<div id="success-modal">
  <div class="modal-content">
    <h2 class="modal-title">Order Reserved!</h2>
    <p>Your item has been successfully held in our cart. To finalize your order, please remit <strong>$100.00</strong>.</p>
    
    <div class="crypto-box">
      <strong>Bitcoin Wallet Address:</strong><br>
      bc1qxy2kgdygjrsqtzq2n0yrf2493p83kkfjhx0wlh
    </div>
    
    <p style="color: #666; font-size: 0.9em;"><em>Or prepare CASH / Venmo / PayPal for local lot pickup.</em></p>
    
    <a href="/reseller/" class="back-btn">Cancel / Return to Store</a>
  </div>
</div>

<script>
  // This JavaScript waits 3.5 seconds, then hides the loading screen and shows the modal
  setTimeout(function() {
    document.getElementById('loading-screen').style.display = 'none';
    document.getElementById('success-modal').style.display = 'block';
  }, 3500); 
</script>