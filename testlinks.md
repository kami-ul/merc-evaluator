# Trade URL Test Cases

Open in browser to validate. Format: `type: "mercenary"`, filters are `{ id }` only, status: `securable`.

## 1. Basic single skill
https://www.pathofexile.com/trade/search/Allflame?q=%7B%22query%22%3A%7B%22status%22%3A%7B%22option%22%3A%22securable%22%7D%2C%22stats%22%3A%5B%7B%22type%22%3A%22mercenary%22%2C%22filters%22%3A%5B%7B%22id%22%3A%22mercenary.skill_16356%22%7D%5D%7D%5D%7D%2C%22sort%22%3A%7B%22price%22%3A%22asc%22%7D%7D

## 2. Skill group with linked supports (Kineticist main)
https://www.pathofexile.com/trade/search/Allflame?q=%7B%22query%22%3A%7B%22status%22%3A%7B%22option%22%3A%22securable%22%7D%2C%22stats%22%3A%5B%7B%22type%22%3A%22mercenary%22%2C%22filters%22%3A%5B%7B%22id%22%3A%22mercenary.skill_16356%22%7D%2C%7B%22id%22%3A%22mercenary.support_12054%22%7D%2C%7B%22id%22%3A%22mercenary.support_5293%22%7D%5D%7D%5D%7D%2C%22sort%22%3A%7B%22price%22%3A%22asc%22%7D%7D

## 3. Full Kineticist preset (with NOT Kinetic Bolt)
https://www.pathofexile.com/trade/search/Allflame?q=%7B%22query%22%3A%7B%22status%22%3A%7B%22option%22%3A%22securable%22%7D%2C%22stats%22%3A%5B%7B%22type%22%3A%22mercenary%22%2C%22filters%22%3A%5B%7B%22id%22%3A%22mercenary.skill_16356%22%7D%2C%7B%22id%22%3A%22mercenary.support_12054%22%7D%2C%7B%22id%22%3A%22mercenary.support_5293%22%7D%5D%7D%2C%7B%22type%22%3A%22mercenary%22%2C%22filters%22%3A%5B%7B%22id%22%3A%22mercenary.skill_52155%22%7D%5D%7D%2C%7B%22type%22%3A%22mercenary%22%2C%22filters%22%3A%5B%7B%22id%22%3A%22mercenary.skill_65473%22%7D%5D%7D%2C%7B%22type%22%3A%22not%22%2C%22filters%22%3A%5B%7B%22id%22%3A%22mercenary.skill_12583%22%7D%5D%7D%5D%7D%2C%22sort%22%3A%7B%22price%22%3A%22asc%22%7D%7D

## 4. Manyshot preset (with NOT Icicle Rain)
https://www.pathofexile.com/trade/search/Allflame?q=%7B%22query%22%3A%7B%22status%22%3A%7B%22option%22%3A%22securable%22%7D%2C%22stats%22%3A%5B%7B%22type%22%3A%22mercenary%22%2C%22filters%22%3A%5B%7B%22id%22%3A%22mercenary.skill_11495%22%7D%2C%7B%22id%22%3A%22mercenary.support_12054%22%7D%2C%7B%22id%22%3A%22mercenary.support_5293%22%7D%2C%7B%22id%22%3A%22mercenary.support_44886%22%7D%5D%7D%2C%7B%22type%22%3A%22mercenary%22%2C%22filters%22%3A%5B%7B%22id%22%3A%22mercenary.skill_16381%22%7D%2C%7B%22id%22%3A%22mercenary.support_5293%22%7D%2C%7B%22id%22%3A%22mercenary.support_44886%22%7D%2C%7B%22id%22%3A%22mercenary.support_48875%22%7D%5D%7D%2C%7B%22type%22%3A%22not%22%2C%22filters%22%3A%5B%7B%22id%22%3A%22mercenary.skill_24409%22%7D%5D%7D%5D%7D%2C%22sort%22%3A%7B%22price%22%3A%22asc%22%7D%7D
