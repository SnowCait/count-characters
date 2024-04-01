<script lang="ts">
	import twitter from 'twitter-text';
	import split from 'graphemesplit';

	let text = '';

	$: characters = split(text);
	$: console.debug(characters, [...text], text.length);

	$: tweet = twitter.parseTweet(text);
</script>

<main>
	<h1>文字数カウント</h1>

	<section>
		<textarea bind:value={text}></textarea>
		<div>
			<button on:click={() => (text = '')}>リセット</button>
		</div>
	</section>

	<section>
		<table>
			<tbody>
				<tr>
					<td>文字数（スペース込み）</td>
					<td><input type="text" value={characters.filter((x) => x !== '\n').length} /></td>
				</tr>
				<tr>
					<td>文字数（スペース無視）</td>
					<td><input type="text" value={characters.filter((x) => !/\s|　/.test(x)).length} /></td>
				</tr>
				<tr>
					<td>行数</td>
					<td><input type="text" value={characters.filter((x) => x === '\n').length + 1} /></td>
				</tr>
				<tr>
					<td>段落数</td>
					<td>
						<input
							type="text"
							value={(text.match(/\n(?:　|\s+|「|『|＜|《|〈|≪|（|“|‘|\(|\"|\')./g)?.length ?? 0) +
								1}
						/>
					</td>
				</tr>
				<tr>
					<td>原稿用紙（400 x ？枚）</td>
					<td>
						<input
							type="text"
							value={Math.ceil(characters.filter((x) => x !== '\n').length / 400)}
						/>
					</td>
				</tr>
				<tr>
					<td>X（Twitter）</td>
					<td>
						<input type="text" value={Math.ceil(tweet.weightedLength / 2)} />
					</td>
				</tr>
			</tbody>
		</table>
	</section>
</main>

<style>
	main {
		text-align: center;
	}

	section {
		margin: 2rem auto;
	}

	textarea {
		width: 100%;
		max-width: 800px;
		height: 20rem;
	}

	table {
		margin: 0 auto;
	}

	td {
		text-align: left;
	}

	input {
		width: 5rem;
	}
</style>
