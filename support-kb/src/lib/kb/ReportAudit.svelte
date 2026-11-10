<script lang="ts">
  import reports from '#lib/data/reports-4471.json';
  import notes from '#lib/data/desk-notes.json';
  import { causeOf, uniqueById, type Report } from '#lib/kb/causes';
  import { tenantOf } from '#lib/kb/locales';

  const rows = (reports as Report[]).filter((report) => report.mergedInto === 4471);
  const unique = uniqueById(rows);
  const mismatches = unique.filter((report) => report.meta.intakeTag !== causeOf(report, tenantOf(report)));
  const outliers = unique.filter((report) => notes.oneDayMove.includes(report.id));
  const mergedJuly2 = unique.filter((report) => report.mergedAt?.startsWith('2026-07-02'));
  const backlog = mergedJuly2.filter((report) => report.receivedAt.slice(0, 10) < '2026-07-02');
</script>

<p>
  The export has <strong>{rows.length}</strong> rows and <strong>{unique.length}</strong>
  distinct reports in #4471. Deduplication by report ID removes
  <strong>{rows.length - unique.length}</strong> repeated rows.
  <strong>{mismatches.length}</strong> intake tags disagree with the tenant-and-channel classification.
</p>

<p>
  On 2 July, <strong>{mergedJuly2.length}</strong> reports were merged;
  <strong>{backlog.length}</strong> had arrived before that date.
  The merge spike is mostly backlog processing, not a sudden spike in newly received reports.
</p>

<h3>Unresolved symptoms within the parser group</h3>
<p>
  The desk notes flag <strong>{outliers.length}</strong> reports as one-day-move readings.
  The tenant-based model files them under <code>parser</code>; that is an assignment,
  not proof of their root cause. Keep them visible for investigation, without subtracting
  them from that group's count or counting them again as a confirmed third problem.
</p>
<ul>
  {#each outliers as report (report.id)}
    <li><code>{report.id}</code> · {report.tenant}: “{report.body}”</li>
  {/each}
</ul>
<p>
  Check the original input, stored date and time, generated invite, timezone and delivery logs.
  Different dates across Lisbon and Toronto suggest a timezone boundary worth checking,
  but the export alone does not establish a timezone defect or explain an invite arriving before booking.
</p>
