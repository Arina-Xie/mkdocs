---
hide:
  - toc
---
# 技术文档翻译1：云数据库只读实例

<blockquote style="margin: 0 0 1rem 0; padding: 0.5rem 1rem; border-left: 4px solid #b91c1c; background-color: #fef2f2;">
  <p style="margin: 0;">本作品展示如何将研发内部的中文笔记，重写为符合技术写作规范的英文用户文档。</p>
  <p style="margin: 0.5rem 0 0 0;">英专生技术写作作品 · 专八/三笔</p>
</blockquote>


## 左右对照

<div style="display: flex; gap: 30px; margin: 30px 0; flex-wrap: wrap; justify-content: center;">

<div style="flex: 1 1 45%; min-width: 320px; background-color: #f9fafc; padding: 28px 30px; border-radius: 20px; border-left: 6px solid #b91c1c; box-shadow: 0 8px 20px rgba(0,0,0,0.03);">

<h3 style="margin: 0 0 0.5rem 0; font-size: 1.5rem; font-weight: 600; color: #0f2b4b;">🔍 原始素材</h3>

<div class="chinese">
        <p><strong>创建与配置云数据库 (RDS) 只读实例</strong></p>
        <p><strong>功能概述：</strong> 只读实例（Read-only Instance）主要用于分担主实例的读取压力。在业务场景出现“读多写少”时，通过创建多个只读实例并配置读写分离连接地址，可以大幅提升数据库系统的整体吞吐量和响应速度。</p>
        <p><strong>应用限制与前提条件：</strong></p>
        <p>
          <ul class="step-list">
            <li>主实例状态： 仅当主实例处于“运行中”状态且未进行存储空间缩容时，方可创建只读实例。</li>
            <li>网络环境： 只读实例必须与主实例处于同一虚拟私有云（VPC）内，以确保内网互通及低延迟。</li>
            <li>数据一致性： 由于采用异步复制机制，只读实例与主实例之间可能存在毫秒级的同步延迟。</li>
          </ul>
        </p>
        <p><strong>配置步骤：</strong></p>
        <p>
          <Ol class="step-list">
            <li>发起创建： 登录数据库管理控制台。在“实例列表”中找到目标主实例，点击其右侧操作列的“更多” > “创建只读实例”。</li>
            <li>规格选型： 
              <ul class="step-list">
                <li>选择只读实例的计算规格（CPU 与内存）。建议只读实例的规格不低于主实例，以防止因同步压力过大导致延迟增加。</li>
                <li>设置存储容量。存储空间将默认与主实例保持一致，且支持随业务增长自动扩容。</li>
              </ul>
            </li>
            <li>配置读写分离：
              <ul class="step-list">
                <li>进入“连接管理”页面，点击“开通读写分离”。</li>
                <li>设置“延迟阈值”。若只读实例的延迟超过该设定值（例如 30 秒），系统将自动停止向该实例分发流量，以防止读取到过期的旧数据。</li>
              </ul>
            </li>
            <li>权重分配： 手动设置各只读实例的流量权重。若权重设为 0，则该实例仅作为备份，不承担前端查询业务。</li>
          </Ol>
        </p>
        <p><strong>安全警告：</strong></p>
        <p>
          <ul class="step-list">
            <li>不可逆操作： 只读实例一旦创建，无法直接将其转换为独立的主实例。</li>
            <li>备份说明： 只读实例不支持开启自动备份功能。若需进行数据恢复，请操作主实例的备份集。</li>
          </ul>
        </p>
        <p><strong>故障排查：</strong></p>
        <p>
          如果发现只读实例状态显示为“同步异常”，请检查主实例是否存在长事务（Long-running transactions）导致日志堆积。建议优化慢查询 SQL 或升级只读实例规格。
        </p>
      </div>

</div>

<div style="flex: 1 1 45%; min-width: 320px; background-color: #ffffff; padding: 28px 30px; border-radius: 20px; border-left: 6px solid #0f6e3f; box-shadow: 0 8px 25px rgba(0,40,20,0.06);">


<h3 style="margin: 0 0 0.5rem 0; font-size: 1.5rem; font-weight: 600; color: #0f2b4b;">✏️ 英文成品</h3>

<div class="english">
        <h3 style="margin-top: 0;">Create and Configure RDS Read-only Instance</h3>
      
        <p><strong>Overview:</strong></p>
        <p>Read-only Instances offload query pressure from the primary instance. In read-heavy scenarios, you can create multiple read-only instances and configure read/write splitting to increase system throughput and response speeds.</p>
        <p><strong>Prerequisites:</strong></p>
        <p>
          <ul class="step-list">
            <li>Status: The primary instance must be in the "running" status. </li>
            <li>Storage: No storage shrinking is in process on the primary instance.</li>
            <li>Network Environment: Read-only Instances and the primary instance must be in the same virtual private cloud (VPC) to ensure internal connectivity and low latency.</li>
            <li>Data Consistency: Due to asynchronous replication mechanism, a synchronization latency of several milliseconds may occur between the read-only Instances and the primary instance.</li>
          </ul>
        </p>
        <p><strong>Configuration Steps:</strong></p>
        <p>
          <Ol class="step-list">
            <li>Initiate Creation: Log in to the RDS console> Find the primary instance in the "instances list">Click "more">"Create a read-only instance".</li>
            <li>Select Specifications: 
              <ul class="step-list">
                <li>Computing: Choose the CPU and memory. Ideally, use a configuration that matches or exceeds the primary instance to prevent latency.</li>
                <li>Storage: By default, storage matches the primary instance and supports Auto-scaling.</li>
              </ul>
            </li>
            <li>Configure Read/Write Splitting:
              <ul class="step-list">
                <li>On the Connection Management page, click "Enable Read/Write Splitting".</li>
                <li>Set a Latency Threshold. If the latency exceeds the threshold (e.g., 30 seconds), the system stops routing traffic to that instance to prevent users from reading stale data. </li>
              </ul>
            </li>
            <li>Assign Weights: Manually set the traffic weight for each instance. A weight of zero indicates that the instance acts as a standby and does not process queries. </li>
          </Ol>
        </p>
        <p><strong>Warning:</strong></p>
        <p>
          <ul class="step-list">
            <li>Irreversible: A read-only instance cannot be promoted to a standalone primary instance.</li>
            <li>Backups: Read-only instances do not support automatic backups. Perform data recovery using the primary instance’s backup sets.</li>
          </ul>
        </p>
        <p><strong>Troubleshooting: </strong></p>
        <p>
          <div>If the instance status is “synchronous exception”, check the primary instance for long-running transactions.</div>
          <div>Suggested Action: Optimize slow SQL query or upgrade the read-only instance's specifications.</div>
        </p>
      </div>

</div>
</div>

