<script lang="ts">
  import { app, save } from "../lib/store.svelte";
  const s = $derived(app.s!);
  const all = $derived(s.channel === "all");
</script>

<h2>输出</h2>
<div class="sub">识别结果如何进入输入框。</div>
<div class="group">
  <div class="row">
    <div class="lbl">边说边上屏<small>{all ? "「多渠道」模式下需先挑选结果，此项不可用" : "识别中的文字实时打到光标处，结束后按定稿修正；关闭后改为结束时一次性粘贴"}</small></div>
    <button class="sw" class:on={s.streaming} disabled={all} onclick={() => save({ streaming: !s.streaming })} aria-label="边说边上屏"></button>
  </div>
  <div class="row">
    <div class="lbl">显示底部悬浮窗<small>录音和识别时在屏幕底部显示状态胶囊；关闭后彻底隐藏</small></div>
    <button class="sw" class:on={s.hudEnabled} onclick={() => save({ hudEnabled: !s.hudEnabled })} aria-label="显示底部悬浮窗"></button>
  </div>
  {#if s.hudEnabled}
    <div class="row">
      <div class="lbl">精简悬浮窗<small>隐藏左侧图标和声波动画，仅保留紧凑提示</small></div>
      <button class="sw" class:on={s.hudMinimal} onclick={() => save({ hudMinimal: !s.hudMinimal })} aria-label="精简悬浮窗"></button>
    </div>
  {/if}
  <div class="row">
    <div class="lbl">胶囊显示实时文字<small>在屏幕底部胶囊中预览识别内容</small></div>
    <button class="sw" class:on={s.liveText} onclick={() => save({ liveText: !s.liveText })} aria-label="胶囊显示实时文字"></button>
  </div>
  <div class="row">
    <div class="lbl">去除标点<small>上屏前删掉所有逗号、句号等标点；小数点、千分位、时间和网址中的符号保留</small></div>
    <button class="sw" class:on={s.stripPunctuation} onclick={() => save({ stripPunctuation: !s.stripPunctuation })} aria-label="去除标点"></button>
  </div>
  <div class="row">
    <div class="lbl">删除句末指定符号<small>只删除整句话结尾的指定符号（保留句中标点），可在右侧自由增删字符</small></div>
    {#if s.stripTrailingEnabled}
      <input
        class="inp"
        style="width: 140px; text-align: center;"
        type="text"
        value={s.stripTrailingChars ?? "，。"}
        placeholder="如：，。！？"
        oninput={(e) => save({ stripTrailingChars: e.currentTarget.value })}
      />
    {/if}
    <button class="sw" class:on={s.stripTrailingEnabled} onclick={() => save({ stripTrailingEnabled: !s.stripTrailingEnabled })} aria-label="删除句末指定符号"></button>
  </div>
  <div class="row">
    <div class="lbl">剪贴板保留策略<small>控制识别结果何时保留在系统剪贴板，防止焦点丢失导致内容蒸发</small></div>
    <select class="sel" value={s.clipboardMode ?? "always"} onchange={(e) => save({ clipboardMode: e.currentTarget.value as "always" | "on_fail" | "on_success" })}>
      <option value="always">所有识别结果都写入剪贴板</option>
      <option value="on_fail">仅未上屏（焦点丢失）时写入</option>
      <option value="on_success">仅上屏成功时写入剪贴板</option>
    </select>
  </div>
  <div class="row">
    <div class="lbl">无输入位置时也强制上屏<small>开启后即使识别不到输入框也会直接粘贴，可能贴到错误位置，默认关闭</small></div>
    <button class="sw" class:on={s.forcePasteWithoutTarget} onclick={() => save({ forcePasteWithoutTarget: !s.forcePasteWithoutTarget })} aria-label="无输入位置时也强制上屏"></button>
  </div>
</div>

<style>
  .inp {
    border: 1px solid var(--line);
    background: var(--card2);
    border-radius: var(--rs);
    padding: 5px 10px;
    color: var(--fg);
    outline: none;
    -webkit-user-select: text;
    user-select: text;
  }
  .inp:focus { border-color: var(--accent); }
</style>
