## Workflow

- When modifying code, follow the engineering workflow defined in the ub-mode SKILL.

## Assets

- Temporarily store all asset files in the `tmp` directory at the project root.
- Export icon assets as SVG and wrap them as components in `@project/components/icons`.
- Export decorative images as PNG at 2x resolution, convert them to WebP using the system-installed `ffmpeg`, and name them with UUIDs in the format `uuid.webp`. Then use the locally installed `ossutil` to upload them to the `tmp` directory in the `gearx` bucket. Credentials such as `accessKeyID` have already been configured in the configuration file.
- Use placeholders for all asset references in code, such as `<img src="uploaded-image-url" />`.

## APIs

- When I explicitly indicate that an API is not yet implemented, mock the relevant endpoints using Next.js API routes, with all mock paths prefixed by `/api/mock`.

## Backward Compatibility

- This project is still under development. Compatibility with legacy data and logic is not required.
