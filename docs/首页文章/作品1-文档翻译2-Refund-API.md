---
hide: 
  - toc
---
<h1>技术文档翻译2：支付网关退款 API</h1>

<blockquote style="margin: 0 0 1rem 0; padding: 0.5rem 1rem; border-left: 4px solid #b91c1c; background-color: #fef2f2;">
  <p style="margin: 0;">本作品展示如何将 API 技术文档从中文翻译为符合国际标准的英文开发者文档。</p>
  <p style="margin: 0.5rem 0 0 0;">英专生技术写作作品 · 专八/三笔</p>
</blockquote>

<hr style="margin: 1.5rem 0; border: none; border-top: 1px solid #dee2e6;">

<h2>左右对照</h2>

<div style="display: flex; gap: 30px; margin: 30px 0; flex-wrap: wrap; justify-content: center;">

  <!-- 左栏：中文原文 -->
  <div style="flex: 1 1 45%; min-width: 320px; background-color: #f9fafc; padding: 28px 30px; border-radius: 20px; border-left: 6px solid #b91c1c; box-shadow: 0 8px 20px rgba(0,0,0,0.03);">

    <h3 style="margin: 0 0 0.5rem 0; font-size: 1.5rem; font-weight: 600; color: #0f2b4b;">🔍 中文原文</h3>
    
    <h4 style="margin: 1rem 0 0.5rem 0; font-size: 1.25rem; font-weight: 600; color: #1e3a5f;">接口名：发起退款请求 (Initiate Refund)</h4>
    
    <p><strong>功能描述</strong><br>
    当订单发生退货或取消时，商户系统需调用本接口向支付网关申请退款。支持部分退款（Partial Refund）和全额退款（Full Refund）。退款资金通常会原路返回至用户的支付账户。</p>
    
    <p><strong>请求信息</strong></p>
    <ul style="margin: 0.5rem 0 1rem 0; padding-left: 1.5rem;">
      <li><strong>URL:</strong> <code style="background-color: #f1f3f5; padding: 2px 6px; border-radius: 4px; font-family: monospace;">https://api.gateway.com/v3/payments/{transaction_id}/refund</code></li>
      <li><strong>Method:</strong> POST</li>
      <li><strong>认证方式：</strong> Bearer Token（需在 HTTP Header 中包含 API Secret Key）</li>
    </ul>
    
    <p><strong>请求参数 (Body)</strong></p>
    
    <table style="width: 100%; border-collapse: collapse; margin: 0.5rem 0 1rem 0; font-size: 14px;">
      <thead>
        <tr style="background-color: #e9ecef;">
          <th style="padding: 8px; text-align: left; border: 1px solid #dee2e6;">参数</th>
          <th style="padding: 8px; text-align: left; border: 1px solid #dee2e6;">类型</th>
          <th style="padding: 8px; text-align: left; border: 1px solid #dee2e6;">必填</th>
          <th style="padding: 8px; text-align: left; border: 1px solid #dee2e6;">说明</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td style="padding: 8px; border: 1px solid #dee2e6;"><code style="background-color: #f1f3f5; padding: 2px 6px; border-radius: 4px;">refund_amount</code></td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">Decimal</td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">是</td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">申请退款的金额。该金额不能大于订单的可退款剩余余额。</td>
        </tr>
        <tr>
          <td style="padding: 8px; border: 1px solid #dee2e6;"><code style="background-color: #f1f3f5; padding: 2px 6px; border-radius: 4px;">reason</code></td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">String</td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">是</td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">退款原因说明，字数限制为 10-200 字。该信息可能会展示给最终用户。</td>
        </tr>
        <tr>
          <td style="padding: 8px; border: 1px solid #dee2e6;"><code style="background-color: #f1f3f5; padding: 2px 6px; border-radius: 4px;">out_refund_no</code></td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">String</td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">是</td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">商户系统内部自定义的退款单号，在商户侧必须保证全局唯一。</td>
        </tr>
        <tr>
          <td style="padding: 8px; border: 1px solid #dee2e6;"><code style="background-color: #f1f3f5; padding: 2px 6px; border-radius: 4px;">notify_url</code></td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">String</td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">否</td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">异步通知地址。退款成功后，系统将通过该 URL 向商户服务器发送回调通知。若不填，则不发送通知。</td>
        </tr>
        <tr>
          <td style="padding: 8px; border: 1px solid #dee2e6;"><code style="background-color: #f1f3f5; padding: 2px 6px; border-radius: 4px;">metadata</code></td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">Object</td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">否</td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">扩展字段。商户可以传递自定义的键值对信息，用于内部账务对齐。</td>
        </tr>
      </tbody>
    </table>
    
    <p><strong>接口逻辑约束</strong></p>
    <ul style="margin: 0.5rem 0 1rem 0; padding-left: 1.5rem;">
      <li><strong>幂等性控制：</strong> 本接口支持幂等性。如果使用相同的 <code style="background-color: #f1f3f5; padding: 2px 6px; border-radius: 4px;">out_refund_no</code> 重复调用，系统将直接返回第一次处理的结果，不会重复扣款。</li>
      <li><strong>退款时效：</strong> 仅支持对 12 个月内创建的订单发起退款，超过该期限的交易请通过线下流程处理。</li>
    </ul>
    
    <p><strong>常见响应状态码</strong></p>
    
    <table style="width: 100%; border-collapse: collapse; margin: 0.5rem 0 1rem 0; font-size: 14px;">
      <thead>
        <tr style="background-color: #e9ecef;">
          <th style="padding: 8px; text-align: left; border: 1px solid #dee2e6;">状态码</th>
          <th style="padding: 8px; text-align: left; border: 1px solid #dee2e6;">说明</th>
        </tr>
      </thead>
      <tbody>
        <tr><td style="padding: 8px; border: 1px solid #dee2e6;">201 Created</td><td style="padding: 8px; border: 1px solid #dee2e6;">退款申请已成功受理。</td></tr>
        <tr><td style="padding: 8px; border: 1px solid #dee2e6;">400 Bad Request</td><td style="padding: 8px; border: 1px solid #dee2e6;">请求参数校验失败（例如金额格式错误）。</td></tr>
        <tr><td style="padding: 8px; border: 1px solid #dee2e6;">403 Forbidden</td><td style="padding: 8px; border: 1px solid #dee2e6;">API Key 无效或权限不足。</td></tr>
        <tr><td style="padding: 8px; border: 1px solid #dee2e6;">409 Conflict</td><td style="padding: 8px; border: 1px solid #dee2e6;">存在冲突的退款请求（例如同一笔交易已有正在处理中的退款）。</td></tr>
        <tr><td style="padding: 8px; border: 1px solid #dee2e6;">422 Unprocessable Entity</td><td style="padding: 8px; border: 1px solid #dee2e6;">余额不足，无法完成退款操作。</td></tr>
      </tbody>
    </table>

  </div>

  <!-- 右栏：英文译文 -->
  <div style="flex: 1 1 45%; min-width: 320px; background-color: #ffffff; padding: 28px 30px; border-radius: 20px; border-left: 6px solid #0f6e3f; box-shadow: 0 8px 25px rgba(0,40,20,0.06);">

    <h3 style="margin: 0 0 0.5rem 0; font-size: 1.5rem; font-weight: 600; color: #0f2b4b;">✏️ 英文译文</h3>
    
    <h4 style="margin: 1rem 0 0.5rem 0; font-size: 1.25rem; font-weight: 600; color: #1e3a5f;">API: Initiate Refund</h4>
    
    <p><strong>Description</strong><br>
    The merchant system uses this API to request a refund from the payment gateway when an order is returned or canceled. The API supports both partial refund and full refund. Refunds are typically returned to the user's original payment account.</p>
    
    <p><strong>Request Information</strong></p>
    <ul style="margin: 0.5rem 0 1rem 0; padding-left: 1.5rem;">
      <li><strong>URL:</strong> <code style="background-color: #f1f3f5; padding: 2px 6px; border-radius: 4px; font-family: monospace;">https://api.gateway.com/v3/payments/{transaction_id}/refund</code></li>
      <li><strong>Method:</strong> POST</li>
      <li><strong>Authentication:</strong> Bearer Token (API Secret Key must be included in the HTTP Header)</li>
    </ul>
    
    <p><strong>Request Parameters (Body)</strong></p>
    
    <table style="width: 100%; border-collapse: collapse; margin: 0.5rem 0 1rem 0; font-size: 14px;">
      <thead>
        <tr style="background-color: #e9ecef;">
          <th style="padding: 8px; text-align: left; border: 1px solid #dee2e6;">Parameter</th>
          <th style="padding: 8px; text-align: left; border: 1px solid #dee2e6;">Type</th>
          <th style="padding: 8px; text-align: left; border: 1px solid #dee2e6;">Required</th>
          <th style="padding: 8px; text-align: left; border: 1px solid #dee2e6;">Description</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td style="padding: 8px; border: 1px solid #dee2e6;"><code style="background-color: #f1f3f5; padding: 2px 6px; border-radius: 4px;">refund_amount</code></td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">Decimal</td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">Yes</td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">The amount to refund. This amount cannot exceed the remaining refundable balance of the order.</td>
        </tr>
        <tr>
          <td style="padding: 8px; border: 1px solid #dee2e6;"><code style="background-color: #f1f3f5; padding: 2px 6px; border-radius: 4px;">reason</code></td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">String</td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">Yes</td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">Description of the refund (10-200 characters). This information may be visible to end users.</td>
        </tr>
        <tr>
          <td style="padding: 8px; border: 1px solid #dee2e6;"><code style="background-color: #f1f3f5; padding: 2px 6px; border-radius: 4px;">out_refund_no</code></td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">String</td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">Yes</td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">A unique identifier for the refund, defined by the merchant system. Must be globally unique within the merchant's system.</td>
        </tr>
        <tr>
          <td style="padding: 8px; border: 1px solid #dee2e6;"><code style="background-color: #f1f3f5; padding: 2px 6px; border-radius: 4px;">notify_url</code></td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">String</td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">No</td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">The URL for asynchronous notification. Upon successful refund, the system sends a callback notification to the merchant server via this URL. If omitted, no notification is sent.</td>
        </tr>
        <tr>
          <td style="padding: 8px; border: 1px solid #dee2e6;"><code style="background-color: #f1f3f5; padding: 2px 6px; border-radius: 4px;">metadata</code></td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">Object</td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">No</td>
          <td style="padding: 8px; border: 1px solid #dee2e6;">A set of key-value pairs used for internal accounting alignment, defined by the merchant.</td>
        </tr>
      </tbody>
    </table>
    
    <p><strong>Constraints</strong></p>
    <ul style="margin: 0.5rem 0 1rem 0; padding-left: 1.5rem;">
      <li><strong>Idempotency:</strong> The API is idempotent. If a request is submitted multiple times with the same <code style="background-color: #f1f3f5; padding: 2px 6px; border-radius: 4px;">out_refund_no</code>, the system returns the initial result and does not process the refund again.</li>
      <li><strong>Refund Window:</strong> The API supports refunds for orders placed within the last 12 months. For orders exceeding this period, use offline processes.</li>
    </ul>
    
    <p><strong>Response Status Codes</strong></p>
    
    <table style="width: 100%; border-collapse: collapse; margin: 0.5rem 0 1rem 0; font-size: 14px;">
      <thead>
        <tr style="background-color: #e9ecef;">
          <th style="padding: 8px; text-align: left; border: 1px solid #dee2e6;">Status Code</th>
          <th style="padding: 8px; text-align: left; border: 1px solid #dee2e6;">Description</th>
        </tr>
      </thead>
      <tbody>
        <tr><td style="padding: 8px; border: 1px solid #dee2e6;">201 Created</td><td style="padding: 8px; border: 1px solid #dee2e6;">The refund request has been successfully processed.</td></tr>
        <tr><td style="padding: 8px; border: 1px solid #dee2e6;">400 Bad Request</td><td style="padding: 8px; border: 1px solid #dee2e6;">Invalid parameters (e.g., incorrect amount format).</td></tr>
        <tr><td style="padding: 8px; border: 1px solid #dee2e6;">403 Forbidden</td><td style="padding: 8px; border: 1px solid #dee2e6;">Invalid API Key or insufficient permissions.</td></tr>
        <tr><td style="padding: 8px; border: 1px solid #dee2e6;">409 Conflict</td><td style="padding: 8px; border: 1px solid #dee2e6;">The request conflicts with the current status of the resource (e.g., a refund for the same order is already in progress).</td></tr>
        <tr><td style="padding: 8px; border: 1px solid #dee2e6;">422 Unprocessable Entity</td><td style="padding: 8px; border: 1px solid #dee2e6;">Insufficient balance to complete the refund.</td></tr>
      </tbody>
    </table>

  </div>

</div>
