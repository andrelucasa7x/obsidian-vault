# SF36 NanoBanana - Image Generation

#ai #imagem #nanobananana #fal-ai

SETUP GUIDE

GENERATE ANY IMAGE
INSIDE CLAUDE CODE
NanoBanana 2 turns Claude Code into a full design studio.
Thumbnails, infographics, ads, product shots, icons, diagrams -all from one prompt. Plus a free library of 1,000+ copy-paste
prompts organized by style.

01

WHAT YOU'LL LEARN

2-command install (MCP server + Claude Code registration)
7 built-in tools: generate, edit, restore, icons, patterns, diagrams, storyboards
1,000+ free prompts organized across 40+ styles and use cases
4K output, text rendering, multi-turn editing, 10 aspect ratios

01

WHY IMAGE
MATTERS

GENERATION

INSIDE

CLAUDE

CODE

Every project needs visuals. Blog posts need featured images. Landing pages need hero graphics. Ad
campaigns need creative variations. Presentations need diagrams. And every time you need one,
you leave your workflow -- open Canva, wait for Midjourney, or message a designer and wait days.
NanoBanana 2 eliminates that. It connects Google's Gemini image models directly to Claude Code
as an MCP server. Once installed, Claude has seven image tools built in. You describe what you want,
Claude generates it, and the image lands in your project directory. No app switching. No waiting. No
separate subscriptions.
And because it runs on Gemini's latest models, the output quality is serious -- 4K resolution, accurate
text rendering, ten different aspect ratios, and multi-turn editing where you describe changes and
Claude updates the image instantly.

02

SETUP (2 COMMANDS)

STEP 1: INSTALL THE MCP SERVER

BUILT WITH CLAUDE CODE | 2026

01

GENERATE ANY IMAGE INSIDE CLAUDE CODE

The NanoBanana MCP server connects Gemini's image generation directly to Claude Code. Install it
globally so Claude can access it from any project.

WANT A FULL CREATIVE WORKFLOW INSIDE CLAUDE CODE?
This guide connects the image tools. The Actionable AI Accelerator shows you how to build
complete creative pipelines -- blog featured images, ad creative generation, social media
graphics, carousel design, and branded asset workflows. Pre-built skills. Step-by-step setup.
Install and run.

whop.com/actionable-ai/standard-3b

npm install -g nanobanana-mcp-server

STEP 2: REGISTER WITH CLAUDE CODE
Tell Claude Code about the new MCP server. This gives Claude access to all seven image tools
automatically.

claude mcp add nanobanana -- node
~/.gemini/extensions/nanobanana/
mcp-server/dist/index.js

STEP 3: SET YOUR API KEY
NanoBanana uses Google's Gemini API for image generation. If you already have a Gemini API key set
up, you're good. Otherwise, grab a free key from Google AI Studio and set it as an environment
variable.

export GEMINI_API_KEY=your_key_here

VERIFY THE CONNECTION
Open Claude Code and ask it to generate a simple test image. If an image appears in your project
directory, you're connected.

BUILT WITH CLAUDE CODE | 2026

02

GENERATE ANY IMAGE INSIDE CLAUDE CODE

PRO MODEL UPGRADE

By default, NanoBanana uses Gemini Flash (fast, good quality). For 4K output and better text
rendering, set NANOBANANA_MODEL=gemini-3-pro-image-preview in your environment.
Higher quality, slightly slower.

BUILT WITH CLAUDE CODE | 2026

03

GENERATE ANY IMAGE INSIDE CLAUDE CODE

03

THE 7 IMAGE TOOLS

Once connected, Claude Code has seven distinct image tools. Each one handles a different type of
visual asset.

1. GENERATE IMAGES
Text-to-image generation with style control. Supports 10 styles (photorealistic, watercolor,
oil-painting, sketch, pixel-art, anime, vintage, modern, abstract, minimalist) and 10 aspect ratios (16:9,
1:1, 4:3, 9:16, and more). Generate up to 8 variations at once with different lighting, angles, color
palettes, or moods.

2. EDIT IMAGES
Modify any existing image with natural language. Say 'make the background darker' or 'remove the
text and add a sunset.' Claude loads the image, applies your changes, and saves the updated
version. Multi-turn editing -- keep refining until it's right.

3. RESTORE IMAGES
Repair and enhance old or damaged photos. Fix scratches, improve resolution, correct colors,
remove noise. Describe what needs fixing and Claude handles the rest.

4. GENERATE ICONS
Create app icons, favicons, and UI elements in multiple sizes (16px to 1024px). Choose flat,
skeuomorphic, minimal, or modern styles. Transparent or colored backgrounds. Rounded or sharp
corners.

5. GENERATE PATTERNS
Create seamless patterns and textures for backgrounds, fabrics, and materials. Geometric, organic,
abstract, floral, or tech styles. Control density (sparse to dense) and color schemes (mono, duotone,
colorful).

6. GENERATE DIAGRAMS
Technical diagrams, flowcharts, architecture diagrams, network maps, wireframes, mind maps, and
sequence diagrams. Professional, clean, hand-drawn, or technical styles. Horizontal, vertical,
hierarchical, or circular layouts.

7. GENERATE STORY SEQUENCES

BUILT WITH CLAUDE CODE | 2026

04

GENERATE ANY IMAGE INSIDE CLAUDE CODE

Create 2-8 sequential images for narratives, tutorials, timelines, or process flows. Consistent or
evolving style across frames. Output as separate files, grid, or comic layout.

BUILT WITH CLAUDE CODE | 2026

05

GENERATE ANY IMAGE INSIDE CLAUDE CODE

04

THE FREE PROMPT LIBRARY (1,000+ PROMPTS)

Writing good image prompts from scratch is hard. This free website solves that. Over 1,000
NanoBanana prompts organized across 40+ use cases and styles. No sign-up required.
youmind.com/nano-banana-pro-prompts

HOW TO USE IT
Browse by use case (thumbnails, infographics, ads, cinematic shots, 3D renders, social posts) or by
style (cyberpunk neon, vintage retro, clean minimal, watercolor, anime). Find a prompt you like, copy
it, paste it into Claude Code, swap in your subject, and generate. Two minutes from idea to finished
image.

BEST USE CASES FROM THE LIBRARY
YouTube thumbnails -- eye-catching designs that pull clicks. Multiple styles from bold and
graphic to cinematic and moody.
Infographics -- visual breakdowns of complex ideas. Clean layouts with data visualization,
process flows, and comparison grids.
Ad creatives -- product shots, lifestyle imagery, and promotional graphics. Styled for social
feeds, stories, and display ads.
Social media posts -- scroll-stopping visuals for Instagram, LinkedIn, and X. Quote graphics,
announcement posts, and content headers.
3D product renders -- photorealistic product shots without a photographer. Perfect for
ecommerce, pitch decks, and marketing.
Cinematic shots -- movie-quality scene compositions. Great for blog headers,
presentation backgrounds, and creative projects.

PROMPT EDITING TIP

Don't just copy prompts as-is. The best workflow: copy a prompt from the library as your base,
swap in your specific subject, then tell Claude to 'generate this and then adjust the lighting to
be warmer.' Multi-turn editing lets you refine without starting over.

BUILT WITH CLAUDE CODE | 2026

06

GENERATE ANY IMAGE INSIDE CLAUDE CODE

05

REAL-WORLD WORKFLOWS

1. BLOG POST FEATURED IMAGES
Writing a blog post in Claude Code? When you're done, tell Claude to generate a featured image
based on the article topic. It reads the content, picks an appropriate visual concept, and generates
the image directly into your blog's image directory. No context switching. The image matches the
content because Claude already understands what you wrote.

2. LANDING PAGE HERO GRAPHICS
Building a landing page? Instead of searching stock photo sites, tell Claude to generate a hero
image that matches your brand and message. Generate multiple variations, pick the best one, and
it's already in your project folder. Adjust colors, swap subjects, or change the mood with follow-up
prompts.

3. AD CAMPAIGN CREATIVE VARIATIONS
Need five ad variations for A/B testing? Generate them all in one session. Same product, different
styles -- photorealistic, minimal, bold graphic, lifestyle, cinematic. Claude generates all five, names
them clearly, and saves them to your project directory. Test which style converts best.

4. CLIENT PRESENTATIONS
Building a pitch deck or proposal? Generate custom diagrams, flowcharts, and visual assets that
match your presentation style. Architecture diagrams for technical proposals. Process flows for
consulting decks. Product mockups for design pitches. All generated on the spot, not pulled from
generic template libraries.

06

PROMPT TEMPLATES

Copy-paste these into Claude Code. Replace bracketed text with your specifics.

YOUTUBE THUMBNAIL

Generate a YouTube thumbnail for a video
about [YOUR TOPIC]. Bold text, high
contrast, eye-catching composition.
16:9 aspect ratio. Make it look like
something you'd click on.

BUILT WITH CLAUDE CODE | 2026

07

GENERATE ANY IMAGE INSIDE CLAUDE CODE

BLOG FEATURED IMAGE

Generate a featured image for this blog
post. Read the content and create a
visual that captures the main concept.
Clean, modern style. 16:9 landscape.
No text on the image.

AD CREATIVE VARIATIONS

Generate 4 ad creative variations for
[YOUR PRODUCT]. Each one should use a
different visual style: photorealistic,
minimal, bold graphic, and lifestyle.
Square format (1:1) for social feeds.

PRODUCT SHOT

Generate a professional product photo of
[YOUR PRODUCT] on a clean background.
Studio lighting. Slight shadow.
Photorealistic style. High resolution.

WANT A FULL CREATIVE PIPELINE INSIDE CLAUDE CODE?
You now have the image tools. The Actionable AI Accelerator shows you how to build automated
creative workflows -- blog image generation, ad creative pipelines, carousel design, social
media graphics, and branded asset systems. Pre-built Claude Code skills. Step-by-step guides.
Install and launch.

whop.com/actionable-ai/standard-3b

BUILT WITH CLAUDE CODE | 2026

08



## Relacionado
- [[SocialAgent]] | [[DesignerAI]] | [[fal-ai-media]]
- [[Design Pipeline]] | [[Content Generation Pipeline]]
