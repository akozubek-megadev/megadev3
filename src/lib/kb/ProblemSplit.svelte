<script lang="ts">
  import reports from '#lib/data/reports-4471.json';
  import { groupByCause, uniqueById, type Report } from '#lib/kb/causes';
  import { tenantOf } from '#lib/kb/locales';

  const ticket = 4471;
  const merged = uniqueById(reports as Report[]).filter((report) => report.mergedInto === ticket);
  const groups = groupByCause(merged, tenantOf);
  const issueByCause: Record<string, string> = {
    parser: 'https://github.com/akozubek-megadev/megadev3/issues/1',
    hint: 'https://github.com/akozubek-megadev/megadev3/issues/2',
  };
  const label: Record<string, string> = {
    parser: 'Shared parser reads month-first',
    hint: 'Booking-v3 form gives a conflicting date hint',
  };
</script>

<p class="summary"><strong>{merged.length}</strong> unique reports in #{ticket} · {reports.length - merged.length} overlapping export rows removed</p>
<table>
  <thead><tr><th>problem</th><th>reports</th><th>ticket</th></tr></thead>
  <tbody>
    {#each groups as group (group.cause)}
      <tr>
        <td>{label[group.cause] ?? group.cause}</td>
        <td>{group.reports.length}</td>
        <td><a href={issueByCause[group.cause]}>#{group.cause === 'parser' ? 1 : 2}</a></td>
      </tr>
    {/each}
  </tbody>
</table>

<style>
  .summary { font-size: 1.1rem; }
  table { border-collapse: collapse; width: 100%; margin: 1rem 0; }
  th, td { border-bottom: 1px solid var(--line); padding: 0.4rem 0.6rem; text-align: left; }
</style>
